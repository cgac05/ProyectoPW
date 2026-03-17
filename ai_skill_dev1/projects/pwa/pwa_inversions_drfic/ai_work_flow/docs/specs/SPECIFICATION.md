# ESPECIFICACIÓN TÉCNICA — v1.0

## Proyecto: Plataforma de Inversiones con IA

**Código del Proyecto**: `pwa_inversions_drfic`
**Categoría**: PWA (Progressive Web App)
**Tech Stack Principal**: React + TypeScript + Vite + TailwindCSS
**Versión**: 1.2
**Fecha**: 2026-03-11
**Autor**: Dr. Francisco Ibarra Carlos
**Estado**: ✅ SPECIFICATION OFICIAL
**Cambios v1.2**: Arquitectura por cores independientes, ranking diario de oportunidades, análisis fundamental/eventos, confluencia configurable y motor de estrategias sobre opciones

---

## 1. Visión General

### 1.1 Objetivo

Desarrollar una **Plataforma Web de Inversiones asistida por Inteligencia Artificial** que permita detectar señales de compra y venta de alta confianza en el mercado de acciones y opciones de EE.UU. (S&P 500 / SPY / QQQ y sus derivados), combinando análisis técnico multicapa (RSI, MACD, Bollinger Bands, EMA/SMA, Volume), análisis de la cadena de opciones, monitoreo de flujo institucional, y confirmación mediante IA (Claude API), todo integrado con Interactive Brokers (IBKR) como broker primario y Alpaca como entorno de desarrollo y paper trading.

### 1.2 Filosofía de la Plataforma

La plataforma opera bajo el modelo **semi-automático**: el cerebro de decisión vive dentro del proyecto como un conjunto de **cores programados y desacoplados**, cada uno especializado en una fuente de verdad distinta. La IA no reemplaza la lógica base; actúa como un core adicional de análisis y sugerencia.

Cada core debe generar señales de compra y venta de forma independiente para el instrumento seleccionado, con su propio score, confianza, razones y contexto. El usuario decide qué cores participan en la decisión final y el sistema solo combina las coincidencias entre los cores activados.

No existe ejecución automática sin intervención humana en v1.0. La plataforma debe:

- descubrir oportunidades diarias de mayor prioridad
- analizar en profundidad el instrumento seleccionado
- combinar coincidencias entre indicadores, estructura técnica, institucionales, noticias, fundamentales e IA
- sugerir la mejor estrategia de opciones para el contexto actual
- permitir la ejecución manual asistida en el broker elegido

---

## 📌 FUENTES DE VERDAD

**SPECIFICATION OFICIAL**: `ai_skill_dev1/projects/pwa/pwa_inversions_drfic/ai_work_flow/docs/specs/SPECIFICATION.md`  
**ÚLTIMA ACTUALIZACIÓN**: March 17, 2026  
**MOTORES BD SELECCIONADOS**: Supabase (principal) + MongoDB (caché)  
**ESTADO**: ✅ Completa y lista para FASE 2.3 (@picoro investigación)

---

### Para la especificación completa, consultar archivo fuente en repositorio.
