# Flutter Template

Template base para proyectos Flutter con configuraciones profesionales pre-configuradas.

## Que incluye

- **GitHub Actions CI** - Analisis y tests automaticos en cada PR
- **Dependabot** - Actualizaciones automaticas de dependencias
- **CLAUDE.md** - Reglas de desarrollo para asistentes IA
- **PR Template** - Formato estandarizado para Pull Requests
- **.gitignore** - Configurado para Flutter con exclusion de secretos

## Como usar este template

### Opcion 1: Desde GitHub Web
1. Click en "Use this template" (boton verde)
2. Nombra tu nuevo repositorio
3. Click en "Create repository"

### Opcion 2: Desde CLI
```bash
gh repo create MI-NUEVO-PROYECTO --template victorleon-dev/flutter-template --public
```

## Despues de crear tu proyecto

1. Clona el repositorio:
```bash
git clone https://github.com/TU-ORG/MI-NUEVO-PROYECTO.git
cd MI-NUEVO-PROYECTO
```

2. Crea el proyecto Flutter:
```bash
flutter create . --org com.tudominio
```

3. Edita `CLAUDE.md` con las reglas especificas de tu proyecto

4. Configura Branch Protection en GitHub:
   - Settings > Branches > Add rule
   - Branch: `main`
   - Require PR + status checks

## Estructura recomendada

```
lib/
  core/           # Utilidades compartidas
  config/         # Configuracion (theme, routes)
  features/       # Features por dominio
    feature_name/
      data/       # Repositorios
      domain/     # Entidades
      presentation/ # UI
```

## Licencia

MIT
