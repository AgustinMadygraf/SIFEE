# SIEF-IA — Sistema de Interpretación de Facturas de Energía Eléctrica

> Proyecto integrador de la **Tecnicatura en Ciencias de Datos e IA**  
> ISFT N°199 El Talar  
> En colaboración con la Tecnicatura Superior en Energía Eléctrica con Orientación en Digitalización

---

## 📖 Descripción

SIEF-IA es una plataforma que permite interpretar automáticamente facturas de electricidad a partir de fotografías, estructurar su información y responder consultas en lenguaje natural a través de web, widget embebido y Telegram.

El proyecto nace como caso de aplicación pedagógico para vincular el dominio energético con las técnicas de Inteligencia Artificial, procesamiento de imágenes y procesamiento del lenguaje natural.

---

## 🏛️ Contexto Institucional

Este repositorio contiene el desarrollo técnico de un proyecto inter-tecnicaturas:

- **Tecnicatura en Ciencias de Datos e IA** (materias 11, 12, 17, 18, 19 y 20)
- **Tecnicatura Superior en Energía Eléctrica con Orientación en Digitalización**

El hilo conductor es la transformación del **dato crudo** (una foto de factura) en **conocimiento estructurado** interpretable por técnicos en energía y consultable por usuarios finales mediante conversación.

---

## 🛠️ Stack Tecnológico

| Capa | Tecnología |
|------|------------|
| **Backend / Web SSR** | Python, FastAPI, Jinja2 |
| **Arquitectura** | Clean Architecture (Dominio, Aplicación, Adaptadores, Infraestructura) |
| **Bot Conversacional** | RASA (NLU + Core) |
| **Canales** | Webapp SSR, Rasa Webchat Widget, Telegram |
| **OCR / Visión** | Tesseract / PaddleOCR (preprocesamiento con OpenCV) |
| **ML / NLP** | scikit-learn, pandas, spaCy (es_core_news_md) |
| **Persistencia** | PostgreSQL, SQLAlchemy, Alembic |
| **Almacenamiento** | Sistema de archivos local / MinIO |
| **Orquestación** | Docker Compose |
| **Tests** | pytest |

---

## 🏗️ Arquitectura de Alto Nivel

El sistema sigue los principios de **Arquitectura Limpia** para garantizar que la lógica de negocio (interpretación de facturas) sea **agnóstica al canal de entrada**:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Web (Jinja2)  │     │  RASA Widget    │     │   Telegram      │
│   FastAPI SSR   │     │   (WebSocket)   │     │   (HTTP Bot)    │
└────────┬────────┘     └────────┬────────┘     └────────┬────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   FastAPI Adapters      │
                    │   (Routers + Controllers)│
                    └────────────┬────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   Application Layer     │
                    │   (Casos de Uso)        │
                    │  • InterpretarFactura   │
                    │  • ConsultarHistorial   │
                    │  • DetectarAnomalia     │
                    └────────────┬────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │      Domain Layer       │
                    │   (Entidades puras)     │
                    │  Factura, Consumo, etc. │
                    └────────────┬────────────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                       │                       │
┌────────▼────────┐     ┌───────▼────────┐     ┌───────▼────────┐
│   OCR Adapter   │     │  Repository    │     │  ML Adapter    │
│ (Tesseract/     │     │  (PostgreSQL)  │     │ (Clasificador, │
│  PaddleOCR)     │     │                │     │  NER, Anomalía)│
└─────────────────┘     └────────────────┘     └────────────────┘
```

---

## 📁 Estructura del Proyecto

```
sief-ia/
├── docker-compose.yml
├── src/
│   └── facturas_ia/
│       ├── domain/          # Entidades, value objects, excepciones, protocolos
│       ├── application/     # Casos de uso y DTOs
│       └── adapters/
│           ├── web/         # FastAPI + Jinja2 (routers, templates, static)
│           ├── persistence/ # SQLAlchemy repositories + migrations
│           ├── ia/          # OCR, ML, NLP adapters
│           └── storage/     # Almacenamiento de imágenes
├── rasa/                    # Proyecto RASA independiente
│   ├── domain.yml
│   ├── actions/
│   └── data/
├── notebooks/               # Experimentación por materia
│   ├── 11-pln/
│   ├── 12-ml/
│   ├── 17-seminario/
│   ├── 18-tpdi/
│   └── 19-modelizado/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
└── docs/
    ├── arquitectura/        # ADRs, diagramas, glosario energético
    └── pedagogia/           # Rúbricas, guías, matrices de vinculación
```

---

## 🗺️ Roadmap

| Fase | Objetivo | Estado |
|------|----------|--------|
| **0. Cimentación** | Definición de dominio, glosario energético, esqueleto Clean Architecture + Docker | 🔲 |
| **1. MVP Web** | Canal web SSR + OCR básico + visualización estructurada | 🔲 |
| **2. Multicanalidad** | Integración RASA (widget + Telegram) + custom actions | 🔲 |
| **3. Inteligencia** | Clasificador de distribuidoras, NER, detección de anomalías, benchmark OCR | 🔲 |
| **4. Producción** | Deploy en VPS, tests E2E, documentación de usuario, presentación conjunta | 🔲 |

---

## 🎓 Materias Vinculadas

| Materia | Componente Técnico |
|---------|-------------------|
| **11 — Técnicas de Procesamiento del Habla** | Motor conversacional RASA (NLU, Core, stories) |
| **12 — Procesamiento de Aprendizaje Automático** | Clasificación de distribuidoras, NER, detección de anomalías |
| **17 — Seminario de Actualización** | Benchmark de OCR, análisis de sesgos, ética de IA |
| **18 — Técnicas de Procesamiento Digital de Imágenes** | Preprocesamiento, segmentación, pipeline OCR |
| **19 — PP: Modelizado de Sistemas de IA** | Arquitectura Limpia, diseño de capas, protocolos |
| **20 — PP: Proyecto Integrador** | Integración end-to-end, deploy, transferencia a usuarios de Energía |

---

## 🚀 Instalación (Entorno de Desarrollo)

> Requisitos: Docker, Docker Compose, Git

```bash
# 1. Clonar
git clone https://github.com/tu-usuario/sief-ia.git
cd sief-ia

# 2. Variables de entorno
cp .env.example .env
# Editar .env con tus credenciales

# 3. Levantar infraestructura
docker-compose up --build

# 4. Ejecutar migraciones
docker-compose exec web alembic upgrade head

# 5. Entrenar RASA (en otra terminal)
docker-compose exec rasa rasa train
```

Accesos:
- Webapp: `http://localhost:8000`
- API docs: `http://localhost:8000/docs`
- RASA: `http://localhost:5005`

---

## 🤝 Cómo Contribuir

1. Fork del repositorio.
2. Crear una rama por feature: `git checkout -b feat/18-preprocesamiento-ocr`.
3. Commits con convención: `feat:`, `fix:`, `docs:`, `refactor:`.
4. Pull Request con descripción técnica y vinculación a la materia correspondiente.

---

## 📄 Licencia

Este proyecto es de carácter académico desarrollado en el ámbito del ISFT N°199 El Talar.  
El código se distribuye bajo licencia MIT para fines educativos.

---

## 📬 Contacto

- **Institución:** ISFT N°199 El Talar
- **Proyecto académico:** Tecnicatura en Ciencias de Datos e IA
- **Dominio aplicado:** Tecnicatura en Energía Eléctrica con Orientación en Digitalización
