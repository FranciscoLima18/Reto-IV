# Salto Innova - Instrucciones para Agentes de IA

## Contexto del Proyecto

**Salto Innova** es un proyecto universitario (Reto IV) para digitalizar la gestión de PYMEs en Salto, Uruguay. El sistema reemplaza procesos manuales/Excel con una plataforma automatizada para tracking de empresas e incubación de negocios.

## Arquitectura y Módulos

### Módulos Principales

- **Módulo Incubadora**: Gestión de etapas (idea → validación → ejecución)
- **Módulo Innovación**: Marketing, finanzas, TICs, transformación digital
- **Módulo Reuniones**: Tipos: primer contacto, seguimiento, derivación/postulación

### Estructura de Datos Clave

Revisar los diagramas MER (Entity-Relationship):

- [MER-saltoInnova.drawio.svg](MER-saltoInnova.drawio.svg)
- [SaltoInnova_RetoIII.svg](SaltoInnova_RetoIII.svg)

Entidades principales: Empresas, Reuniones, Usuarios/Funcionarios, Programas, Instrumentos

## Requerimientos Específicos

### Funcionalidades Core

1. **Registro y seguimiento de PYMEs**
   - Datos críticos: RUT, facturación, cantidad de empleados
   - Historial multi-año para métricas de evolución
   - Sistema de tagueo para categorizar problemas

2. **Gestión de Reuniones**
   - Integración con Google Calendar (prioridad alta)
   - Tipos: primer contacto, seguimiento, derivación a instrumentos
   - Variables: presencial, telefónico, online, en empresa

3. **Sistema de Recordatorios**
   - WhatsApp y email para seguimiento activo
   - Las empresas tienden a "dejarse estar" según stakeholders

4. **Roles y Permisos**
   - Multi-usuario: cada funcionario tiene cuenta propia
   - Acceso diferenciado según rol
   - NO requiere validez de declaración jurada

### Requisitos No Funcionales

- **Seguridad y Confidencialidad**: Prioridad CRÍTICA
- **UI Intuitiva**: Pantalla de login, flujo unificado (no fragmentado)
- **Modularidad**: Sistema escalable por módulos, estático pero extensible
- **Automatización**: IDs automáticos, eliminación de trabajo manual

## Documentación de Referencia

Consultar para contexto detallado:

- [Entrevista a Pablo - UCU.txt](Entrevista a Pablo - UCU.txt) - Requerimientos del stakeholder
- [Notas entrevista PRUEBA.md](Notas entrevista PRUEBA.md) - Misión, visión, desafíos
- [SaltoInnovaSRS.pdf](SaltoInnovaSRS.pdf) - Especificación de requerimientos
- [BE_BUC_PUC_SaltoInnova.pdf](BE_BUC_PUC_SaltoInnova.pdf) - Business case

## Convenciones del Proyecto

### Idioma

- **Código y comentarios**: Español (proyecto universitario local)
- **Documentación**: Español
- **Variables/funciones**: Preferir nombres descriptivos en español

### Terminología Específica

- **PYME**: Pequeña y Mediana Empresa (≤ $1M USD facturación anual)
- **POA**: Plan Operativo Anual
- **Autodiagnóstico**: Formulario inicial para empresas
- **Cliente vs Pre-Cliente**: Estado de madurez en el sistema

## Flujo de Trabajo del Usuario

1. Empresa se acerca a Salto Innova (presencial/online/teléfono)
2. Primer contacto formal → Registro de datos
3. Autodiagnóstico → Recomendaciones personalizadas
4. Reuniones de seguimiento → Sistema de tagueo
5. Plan de acción según necesidades (incubadora/innovación)
6. Tracking continuo con recordatorios automáticos

## Consideraciones de Implementación

- **NO eliminar interacción humana**: El sistema asiste, no reemplaza
- **Evitar desperdicio de tiempo en relevamiento**: Automatizar captura de datos
- **Fin de ciclo**: Empresa cierra o se expande (mantener historial)
- **Métricas históricas**: Base para futuras gestiones administrativas

## Integraciones Planificadas

- **Google Calendar** (mandatorio): Agendar reuniones, multi-usuario
- **API de conferencias** (a investigar): Para reuniones virtuales
- Notificaciones: WhatsApp, Email

---

_Última actualización: Basada en entrevistas y documentación del Reto IV (2026)_
