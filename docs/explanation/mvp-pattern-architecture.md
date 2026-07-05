# Explanation: Model-View-Presenter (MVP) Pattern Architecture

This document describes how to implement the Model-View-Presenter (MVP) pattern inside our Unity project to enforce the Single Responsibility Principle (SRP) and keep business/simulation logic isolated from the rendering engine.

---

## 1. Architectural Rules for Segregation

To maintain clean separation and prevent architectural drift, all code must follow these rules:

### Rule 1: Models Must Be Pure C#
*   **No Unity Dependencies:** Model files must not include `using UnityEngine;` or inherit from `MonoBehaviour`.
*   **State & Math Only:** Models store variables (data) and compute state transformations (e.g., physics, orbit math, resource accumulation).
*   **Notification via Events:** Models communicate state changes outward exclusively using standard C# events or `System.Action`. They must have zero knowledge of Views or Presenters.

### Rule 2: Views Are Dumb Visual Translators
*   **No Business Logic:** Views must never compute physics, modify raw state variables, or calculate simulation steps.
*   **Engine & Components:** Views inherit from `MonoBehaviour`. They manage assets like UI canvas elements, particle effects, meshes, sound clips, and animation controllers.
*   **Input Collection:** Views collect raw hardware inputs (keys, buttons, clicks) and expose properties or basic methods for the Presenter to poll or listen to. They must never directly call methods on the Model.

### Rule 3: Presenters Are the Direct Brokers
*   **Mediator Role:** Presenters bind Models to Views. They instantiate or reference the Model, subscribe to its state events, and update the View.
*   **Thin Logic Layer:** Presenters handle frame-by-frame updates (`Update()`, `FixedUpdate()`), poll View inputs, trigger Model updates, and delegate all rendering details to the View.

---

## 2. Directory Hierarchy Reference

Keep scripts organized in separate directories:
```text
Assets/
└── Scripts/
    ├── Simulation/       # Models (Pure C# logic)
    ├── UI/               # Views (MonoBehaviours and UI interfaces)
    └── Controllers/      # Presenters (Mediators)
```

---

## 3. Reference Implementation: Lunar Lander

### A. The Model (`Assets/Scripts/Simulation/LanderModel.cs`)
```csharp
using System;

namespace Moonfall.Simulation
{
    public class LanderModel
    {
        public float Altitude { get; private set; }
        public float Velocity { get; private set; }
        public float Fuel { get; private set; }
        
        public const float Gravity = -1.62f;
        public const float ThrustForce = 3.0f;
        public const float FuelBurnRate = 10f;

        public event Action<float, float, float> OnStateChanged;

        public LanderModel(float startAltitude, float startFuel)
        {
            Altitude = startAltitude;
            Fuel = startFuel;
            Velocity = 0f;
        }

        public void UpdatePhysics(float deltaTime, bool isThrusting)
        {
            float acceleration = Gravity;

            if (isThrusting && Fuel > 0)
            {
                acceleration += ThrustForce;
                Fuel = Math.Max(0, Fuel - (FuelBurnRate * deltaTime));
            }

            Velocity += acceleration * deltaTime;
            Altitude = Math.Max(0, Altitude + (Velocity * deltaTime));

            OnStateChanged?.Invoke(Altitude, Velocity, Fuel);
        }
    }
}
```

### B. The View (`Assets/Scripts/UI/LanderView.cs`)
```csharp
using UnityEngine;
using UnityEngine.UI;

namespace Moonfall.UI
{
    public class LanderView : MonoBehaviour
    {
        [SerializeField] private Text altitudeText;
        [SerializeField] private Text velocityText;
        [SerializeField] private Text fuelText;
        [SerializeField] private ParticleSystem thrusterParticles;
        [SerializeField] private Transform landerTransform;

        public bool IsThrustKeyPressed() => Input.GetKey(KeyCode.Space);

        public void DisplayState(float altitude, float velocity, float fuel)
        {
            altitudeText.text = $"Altitude: {altitude:F1} m";
            velocityText.text = $"Velocity: {velocity:F1} m/s";
            fuelText.text = $"Fuel: {fuel:F0}%";

            Vector3 pos = landerTransform.position;
            pos.y = altitude;
            landerTransform.position = pos;
        }

        public void SetThrusterVFX(bool active)
        {
            if (active && !thrusterParticles.isPlaying)
                thrusterParticles.Play();
            else if (!active && thrusterParticles.isPlaying)
                thrusterParticles.Stop();
        }
    }
}
```

### C. The Presenter (`Assets/Scripts/Controllers/LanderPresenter.cs`)
```csharp
using UnityEngine;
using Moonfall.Simulation;
using Moonfall.UI;

namespace Moonfall.Controllers
{
    public class LanderPresenter : MonoBehaviour
    {
        [SerializeField] private LanderView view;
        private LanderModel _model;

        private void Start()
        {
            _model = new LanderModel(100f, 100f);
            _model.OnStateChanged += UpdateView;
            UpdateView(_model.Altitude, _model.Velocity, _model.Fuel);
        }

        private void Update()
        {
            bool isThrusting = view.IsThrustKeyPressed();
            _model.UpdatePhysics(Time.deltaTime, isThrusting);
            view.SetThrusterVFX(isThrusting && _model.Fuel > 0);
        }

        private void UpdateView(float altitude, float velocity, float fuel)
        {
            view.DisplayState(altitude, velocity, fuel);
        }

        private void OnDestroy()
        {
            if (_model != null)
                _model.OnStateChanged -= UpdateView;
        }
    }
}
```
