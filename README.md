# Simulador de flujo de potencia AC

Simulador interactivo para ver cómo se reparte la potencia activa en una red de corriente alterna mallada, y cómo cambia ese reparto al actuar sobre la generación, la topología, la impedancia de las líneas o un transformador desfasador.

**[Abrir el simulador](https://ajrcjf-web.github.io/ac-power-flow-simulator/)** · **[Leer la guía de conceptos (PDF)](https://ajrcjf-web.github.io/ac-power-flow-simulator/docs/flujo_potencia_AC_conceptos.pdf)**

![Captura del simulador](docs/captura.png)

## Objetivo

En una línea AC la potencia activa depende de las tensiones en los extremos, la reactancia y el ángulo entre ambos nodos:

```
P ≈ V1 · V2 · sen δ / X
```

Nadie ajusta ese flujo directamente: se reparte por la red según las impedancias. Para cambiarlo hay que actuar de forma indirecta. Este proyecto busca que esa idea se entienda viéndola, sin necesidad de experiencia previa en herramientas de simulación de sistemas de potencia.

Está pensado para ingenieros, responsables de proyecto y managers con formación eléctrica que quieran una intuición rápida de por qué aliviar una línea casi siempre carga otra.

## Qué incluye

| Archivo | Contenido |
|---|---|
| `index.html` | El simulador completo en un solo archivo. No requiere instalación ni servidor. |
| `docs/flujo_potencia_AC_conceptos.pdf` | Guía de 6 páginas con los conceptos, las cuatro palancas y ejemplos numéricos tomados del simulador. |

## Funcionamiento básico

La red tiene 5 nodos y 7 líneas. La generación principal está en Norte y la demanda principal en Sur. En el caso base la línea B queda al 104 % de su límite: ese es el problema por resolver.

Palancas disponibles:

| Palanca | Control en el simulador | Qué representa |
|---|---|---|
| Redespacho | Deslizadores de G2 y G3 (G1 cubre la diferencia) | Cambiar dónde se genera |
| Topología | Casilla de cada línea | Abrir o cerrar interruptores |
| Impedancia | Deslizador de reactancia por línea, 50 a 200 % | Compensación serie o reactor serie |
| Desfasador | Ángulo φ de −15° a +15° en la línea B | Transformador desfasador (PST) |

El diagrama muestra sentido, MW y porcentaje de carga de cada línea. La tabla inferior compara cada flujo con una referencia que se puede fijar en cualquier momento, para ver el efecto incremental de cada acción. Hay cinco botones de experimento, uno por palanca más el caso base.

### Modelo

Flujo de cargas linealizado (flujo DC) sobre base 100 MVA. Por cada línea:

```
P ≈ (δ1 − δ2 + φ) / X      [pu]
```

Se resuelve el sistema lineal B·δ = P con el nodo Norte como referencia (δ = 0) y nodo de balance.

## Consideraciones y límites

- **Es una herramienta didáctica, no de estudio.** Los datos de la red son ilustrativos y están elegidos para que los efectos se vean con claridad. No representan una red real.
- **Aproximación DC.** Sin pérdidas, tensiones en 1 pu y ángulos pequeños (sen δ ≈ δ). Es adecuada para el reparto de potencia activa; no calcula potencia reactiva, perfil de tensiones ni pérdidas.
- **Solo evalúa el límite térmico.** No hay análisis de estabilidad ni de tensión, ni comprobación N-1 automática.
- **Islas.** Si una maniobra aísla un nodo del nodo de balance, el modelo lo considera desenergizado.
- **Sin HVDC.** Un enlace HVDC, donde la potencia sí se fija por consigna, no está modelado. La guía en PDF lo comenta como contraste.

Para estudios reales se usan herramientas de flujo de cargas AC completo con datos validados de la red.

## Uso local

Descarga `index.html` y ábrelo en cualquier navegador moderno. Necesita conexión solo para cargar las tipografías; sin ella funciona igual con las del sistema.

## Créditos

Desarrollado con asistencia de Claude (Anthropic).

## Licencia

MIT. Ver [LICENSE](LICENSE).
