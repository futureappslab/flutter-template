# 📱 Convenciones de Flutter para este proyecto

> Detalle específico de Flutter. Las reglas de ingeniería generales están en `AGENTS.md`.

## 1. Arquitectura del Proyecto

<!-- Describir la estructura de carpetas y arquitectura usada -->

```
lib/
  core/           # Utilidades compartidas
  config/         # Configuracion (theme, routes, etc.)
  features/       # Features por dominio
    feature_name/
      data/       # Repositorios, datasources
      domain/     # Entidades, casos de uso
      presentation/ # Screens, widgets, providers
```

---

## 2. Convenciones de Codigo

### Nombrado
- Clases: `PascalCase`
- Variables/funciones: `camelCase`
- Archivos: `snake_case.dart`
- Constantes: `SCREAMING_SNAKE_CASE`

### Widgets
- Un widget por archivo cuando es complejo
- Widgets pequeños pueden agruparse

---

## 3. Base de Datos

<!-- Configuracion de Supabase u otra DB si aplica -->

---

## 4. Testing

```bash
flutter test                    # Todos los tests
flutter test test/unit/         # Solo unit tests
flutter test --coverage         # Con cobertura
```

---

## 5. Comandos Utiles

```bash
flutter pub get                 # Instalar dependencias
flutter analyze                 # Analizar codigo
flutter run                     # Ejecutar app
flutter build apk               # Build Android
flutter build ios               # Build iOS
```
