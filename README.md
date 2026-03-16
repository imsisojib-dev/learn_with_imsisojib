# Flutter Project Architecture

A scalable, Clean Architecture–based Flutter project structure using BLoC for state management and GetIt for dependency injection.

---

## Layer Overview

```
lib/
├── main.dart
├── app.dart                          # MaterialApp + root widget
├── core/                             # Platform infrastructure (no Flutter UI)
├── config/                           # App-wide configuration & wiring
├── shared/                           # Reusable UI, utilities & cross-feature logic
└── features/                         # Feature modules (Clean Architecture)
```

> **Dependency rule:** `features` → `shared` → `config` → `core`
> No feature imports from another feature. Cross-feature communication goes through `shared/cubits/` or route navigation.

---

## Full Structure

```
lib/
├── main.dart
├── app.dart
│
├── core/                             # Pure infrastructure — no Flutter widgets
│   ├── network/
│   │   ├── api_client.dart
│   │   ├── dio_client.dart
│   │   ├── network_info.dart
│   │   ├── interceptors/
│   │   │   ├── auth_interceptor.dart
│   │   │   ├── logging_interceptor.dart
│   │   │   ├── error_interceptor.dart
│   │   │   └── retry_interceptor.dart
│   │   └── response/
│   │       ├── api_response.dart
│   │       └── base_response.dart
│   │
│   ├── local/
│   │   ├── shared_preferences_service.dart
│   │   ├── secure_storage_service.dart
│   │   ├── hive_service.dart
│   │   ├── sqflite_service.dart
│   │   └── cache_manager.dart
│   │
│   ├── error/
│   │   ├── failures.dart
│   │   ├── exceptions.dart
│   │   ├── error_handler.dart
│   │   └── app_error.dart
│   │
│   ├── security/
│   │   ├── encryption.dart
│   │   ├── biometric_auth.dart
│   │   ├── certificate_pinning.dart
│   │   └── secure_storage.dart
│   │
│   ├── platform/
│   │   ├── method_channel_manager.dart
│   │   ├── event_channel_manager.dart
│   │   └── platform_interface.dart
│   │
│   ├── logging/
│   │   ├── logger.dart
│   │   ├── log_filter.dart
│   │   └── crash_reporter.dart
│   │
│   ├── analytics/
│   │   ├── analytics_service.dart
│   │   ├── event_tracker.dart
│   │   └── analytics_events.dart
│   │
│   ├── notifications/
│   │   ├── notification_service.dart
│   │   ├── local_notification_service.dart
│   │   └── fcm_service.dart
│   │
│   ├── location/
│   │   ├── location_service.dart
│   │   └── geolocation_helper.dart
│   │
│   ├── connectivity/
│   │   ├── connectivity_service.dart
│   │   └── network_monitor.dart
│   │
│   ├── media/
│   │   ├── image_picker_service.dart
│   │   ├── video_picker_service.dart
│   │   ├── file_picker_service.dart
│   │   ├── camera_service.dart
│   │   └── image_compressor.dart
│   │
│   ├── payment/
│   │   ├── payment_service.dart
│   │   ├── stripe_service.dart
│   │   ├── razorpay_service.dart
│   │   └── ssl_commerz_service.dart
│   │
│   ├── deeplink/
│   │   ├── deeplink_service.dart
│   │   └── deeplink_handler.dart
│   │
│   ├── background/
│   │   ├── background_service.dart
│   │   └── workmanager_service.dart
│   │
│   ├── enums/
│   │   ├── app_enums.dart
│   │   ├── error_type.dart
│   │   ├── loading_state.dart
│   │   └── network_status.dart
│   │
│   └── mixins/
│       ├── validation_mixin.dart
│       ├── loading_mixin.dart
│       └── lifecycle_mixin.dart
│
│
├── config/                           # App wiring — routes, DI, environment, i18n
│   ├── app_config.dart
│   ├── flavor_banner.dart
│   ├── app_initializer.dart
│   │
│   ├── routes/
│   │   ├── app_router.dart
│   │   ├── route_generator.dart
│   │   ├── route_paths.dart
│   │   ├── route_guards.dart
│   │   └── route_transitions.dart
│   │
│   ├── di/
│   │   ├── injection_container.dart
│   │   ├── injection_container.config.dart  # Generated
│   │   ├── service_locator.dart
│   │   └── modules/
│   │       ├── network_module.dart
│   │       ├── storage_module.dart
│   │       ├── service_module.dart
│   │       └── repository_module.dart
│   │
│   ├── environment/
│   │   ├── environment.dart
│   │   ├── env_config.dart
│   │   ├── flavor_config.dart
│   │   ├── dev_config.dart
│   │   ├── staging_config.dart
│   │   ├── prod_config.dart
│   │   └── env.dart                  # Generated by envied
│   │
│   ├── localization/
│   │   ├── app_localizations.dart
│   │   ├── localization_service.dart
│   │   └── languages/
│   │       ├── en.json
│   │       ├── bn.json
│   │       └── ar.json
│   │
│   ├── observers/
│   │   ├── bloc_observer.dart
│   │   ├── route_observer.dart
│   │   └── lifecycle_observer.dart
│   │
│   ├── permissions/
│   │   ├── permission_config.dart
│   │   └── permission_handler.dart
│   │
│   ├── features/
│   │   ├── feature_flags.dart
│   │   └── feature_config.dart
│   │
│   └── middleware/
│       ├── auth_middleware.dart
│       └── analytics_middleware.dart
│
│
├── shared/                           # Flutter-aware utilities shared across features
│   ├── theme/                        # UI theming — Flutter widget layer
│   │   ├── bloc/
│   │   │   └── theme_bloc.dart
│   │   ├── app_theme.dart
│   │   ├── app_colors.dart
│   │   ├── app_typography.dart
│   │   └── app_theme_extension.dart
│   │
│   ├── widgets/                      # Reusable UI components
│   │   ├── buttons/
│   │   │   ├── primary_button.dart
│   │   │   ├── secondary_button.dart
│   │   │   ├── icon_button.dart
│   │   │   └── loading_button.dart
│   │   │
│   │   ├── inputs/
│   │   │   ├── app_text_field.dart
│   │   │   ├── search_field.dart
│   │   │   ├── password_field.dart
│   │   │   └── otp_field.dart
│   │   │
│   │   ├── dialogs/
│   │   │   ├── confirm_dialog.dart
│   │   │   ├── alert_dialog.dart
│   │   │   └── bottom_sheet_base.dart
│   │   │
│   │   ├── feedback/
│   │   │   ├── loading_overlay.dart
│   │   │   ├── shimmer_loader.dart
│   │   │   ├── empty_state.dart
│   │   │   └── error_view.dart
│   │   │
│   │   ├── layout/
│   │   │   ├── app_scaffold.dart
│   │   │   ├── app_bar_widget.dart
│   │   │   ├── responsive_builder.dart
│   │   │   └── sliver_app_bar_delegate.dart
│   │   │
│   │   ├── media/
│   │   │   ├── cached_image.dart
│   │   │   ├── avatar_widget.dart
│   │   │   └── video_player_widget.dart
│   │   │
│   │   └── misc/
│   │       ├── badge_widget.dart
│   │       ├── divider_widget.dart
│   │       └── pagination_widget.dart
│   │
│   ├── extensions/
│   │   ├── string_extensions.dart
│   │   ├── context_extensions.dart
│   │   ├── date_extensions.dart
│   │   ├── int_extensions.dart
│   │   ├── double_extensions.dart
│   │   ├── list_extensions.dart
│   │   ├── widget_extensions.dart
│   │   └── color_extensions.dart
│   │
│   ├── constants/
│   │   ├── app_constants.dart
│   │   ├── api_constants.dart
│   │   ├── asset_constants.dart
│   │   ├── storage_keys.dart
│   │   ├── route_constants.dart
│   │   └── string_constants.dart
│   │
│   ├── utils/
│   │   ├── validators.dart
│   │   ├── formatters.dart
│   │   ├── date_utils.dart
│   │   ├── currency_utils.dart
│   │   ├── string_utils.dart
│   │   ├── file_utils.dart
│   │   ├── image_utils.dart
│   │   ├── encryption_utils.dart
│   │   └── math_utils.dart
│   │
│   ├── helpers/
│   │   ├── dialog_helper.dart
│   │   ├── snackbar_helper.dart
│   │   ├── permission_helper.dart
│   │   ├── share_helper.dart
│   │   ├── url_launcher_helper.dart
│   │   └── device_info_helper.dart
│   │
│   ├── cubits/                       # Cross-feature app-wide state
│   │   ├── app_cubit.dart
│   │   ├── connectivity_cubit.dart
│   │   └── locale_cubit.dart
│   │
│   └── models/                       # Shared domain models (not feature-specific)
│       ├── pagination_model.dart
│       ├── address_model.dart
│       └── media_model.dart
│
│
└── features/                         # Feature modules — each follows Clean Architecture
    │
    ├── auth/                         # ── Canonical example (all features mirror this) ──
    │   ├── data/
    │   │   ├── datasources/
    │   │   │   ├── auth_remote_datasource.dart
    │   │   │   └── auth_local_datasource.dart
    │   │   ├── models/
    │   │   │   ├── user_model.dart
    │   │   │   └── token_model.dart
    │   │   └── repositories/
    │   │       └── auth_repository_impl.dart
    │   │
    │   ├── domain/
    │   │   ├── entities/
    │   │   │   └── user_entity.dart
    │   │   ├── repositories/
    │   │   │   └── auth_repository.dart      # Abstract
    │   │   └── usecases/
    │   │       ├── login_usecase.dart
    │   │       ├── logout_usecase.dart
    │   │       └── refresh_token_usecase.dart
    │   │
    │   └── presentation/
    │       ├── bloc/
    │       │   ├── auth_bloc.dart
    │       │   ├── auth_event.dart
    │       │   └── auth_state.dart
    │       ├── pages/
    │       │   ├── login_page.dart
    │       │   ├── register_page.dart
    │       │   └── otp_page.dart
    │       └── widgets/
    │           ├── login_form.dart
    │           └── social_login_buttons.dart
    │
    ├── home/                         # data/ domain/ presentation/
    ├── profile/                      # data/ domain/ presentation/
    ├── products/                     # data/ domain/ presentation/
    ├── cart/                         # data/ domain/ presentation/
    ├── orders/                       # data/ domain/ presentation/
    ├── checkout/                     # data/ domain/ presentation/
    ├── notifications/                # data/ domain/ presentation/
    └── settings/                     # data/ domain/ presentation/
```

---

## Layer Responsibilities

### `core/` — Infrastructure
Pure Dart/platform services. No Flutter widgets. No business logic.
Files here could run in a Dart CLI or backend project.

| Sub-folder | Responsibility |
|---|---|
| `network/` | Dio setup, interceptors, base response models |
| `local/` | SharedPreferences, Hive, SQLite, secure storage |
| `error/` | Failures, exceptions, global error handler |
| `security/` | Encryption, biometrics, certificate pinning |
| `platform/` | MethodChannel / EventChannel bridges |
| `logging/` | App logger, log filters, crash reporter |
| `analytics/` | Analytics service, event tracker |
| `notifications/` | FCM, local notifications |
| `media/` | Image/video/file pickers, camera, compressor |
| `payment/` | Stripe, Razorpay, SSLCommerz integrations |
| `deeplink/` | Deep link handling |
| `background/` | Background tasks, WorkManager |

---

### `config/` — App Wiring
Bootstraps and connects everything. Runs once at startup.

| Sub-folder | Responsibility |
|---|---|
| `routes/` | GoRouter / Navigator 2 setup, guards, transitions |
| `di/` | GetIt injection container, all module registrations |
| `environment/` | Dev / staging / prod flavors, envied-generated env |
| `localization/` | ARB/JSON strings, locale service |
| `observers/` | BlocObserver, RouteObserver, LifecycleObserver |
| `permissions/` | Permission request configuration |
| `features/` | Feature flag definitions and remote config |
| `middleware/` | Auth and analytics middleware |

---

### `shared/` — Reusable Flutter Layer
Flutter-aware code shared across features. Imports `package:flutter`.

| Sub-folder | Responsibility |
|---|---|
| `theme/` | ThemeData, colors, typography, ThemeExtension, ThemeBloc |
| `widgets/` | All reusable UI components (buttons, inputs, dialogs, etc.) |
| `extensions/` | Dart/Flutter extension methods |
| `constants/` | App-wide constant values |
| `utils/` | Stateless utility/helper functions |
| `helpers/` | Context-dependent helpers (dialogs, snackbars, etc.) |
| `cubits/` | Cross-feature state (app, connectivity, locale) |
| `models/` | Shared domain models not owned by any single feature |

---

### `features/` — Feature Modules
Each feature is fully self-contained and follows Clean Architecture layers.

```
feature_name/
├── data/                 # Implementation details
│   ├── datasources/      # Remote (API) and local (cache) sources
│   ├── models/           # JSON-serializable data models (extends entities)
│   └── repositories/     # Repository implementations
│
├── domain/               # Business rules — pure Dart, no Flutter, no packages
│   ├── entities/         # Core business objects
│   ├── repositories/     # Abstract repository contracts
│   └── usecases/         # Single-responsibility use cases
│
└── presentation/         # UI layer
    ├── bloc/             # BLoC: events, states, logic
    ├── pages/            # Full screens registered in the router
    └── widgets/          # Widgets used only within this feature
```

> **Rule:** `domain/` has zero external dependencies — no Dio, no Hive, no Flutter.
> `data/` implements `domain/` contracts. `presentation/` calls `domain/` use cases only.

---

## Key Design Decisions

- **`theme/` lives in `shared/`**, not `core/` — it imports `package:flutter/material.dart` and is a UI concern, not infrastructure.
- **Features never import each other** — shared state goes through `shared/cubits/`, navigation goes through `config/routes/`.
- **`domain/` is framework-free** — use cases and entities are plain Dart classes, making them trivially testable.
- **One BLoC per feature** for complex features; a lightweight `Cubit` is preferred for simpler state.
