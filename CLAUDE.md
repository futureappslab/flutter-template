# Reglas de Desarrollo - [NOMBRE_PROYECTO]

## 0. Reglas Fundamentales

### 0.1 Una feature = Una rama
- Cada tarea nueva debe tener su propia rama
- Crear desde `main` siempre actualizado
- Nombre: `feature/`, `fix/`, `docs/`, `chore/`

### 0.2 Flujo Git obligatorio
```bash
git checkout main
git pull origin main
git checkout -b feature/nombre-descriptivo
```

### 0.3 Commits convencionales
- `feat:` nueva funcionalidad
- `fix:` correcciones
- `docs:` documentacion
- `chore:` mantenimiento
- `refactor:` mejoras sin cambiar funcionalidad

### 0.4 Pull Requests
- Siempre crear PR para merge a main
- Esperar que CI pase antes de merge
- Descripcion clara de los cambios

### 0.5 Revisar ramas pendientes
Antes de crear una nueva rama, verificar si hay trabajo pendiente:
```bash
git branch -a
```

### 0.6 Dar recomendaciones
Al ofrecer opciones, incluir cual es la recomendada y por que.

---

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
