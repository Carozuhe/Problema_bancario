🏦 Simulación de Colas M/M/1 — Banco de Colombia

Laboratorio de simulación de eventos discretos para optimizar la distribución de cajeros en un banco sin cajeros electrónicos.
Materia: Simulación de Sistemas · Ingeniería de Software y Datos


📋 Descripción del problema
El Banco de Colombia opera con tres cajeros humanos que atienden dos tipos de transacciones: retiros y pagos/consignaciones. El banco quería saber cómo distribuir esas cajas para minimizar los tiempos de espera de sus clientes.
Se evaluaron cuatro configuraciones:
EscenarioDescripciónMixto 3 cajerosCualquier cajero atiende cualquier operación (configuración actual)1R + 2P1 caja exclusiva para retiros, 2 para pagos2R + 1P2 cajas para retiros, 1 para pagosMixto 4 cajerosEvalúa si es necesario agregar un cajero

⚙️ Modelo
Cada cajero se implementa como un sistema M/M/1 independiente:

Llegadas: proceso de Poisson (tiempos entre llegadas exponenciales)
Servicio: distribución exponencial
Disciplina: cola de prioridad no apropiativa (Rápido > Normal > Lento > Muy lento), FIFO dentro de cada nivel
Horizonte de simulación: 480 minutos (8 horas de operación)
Réplicas: 10 por escenario, con semillas independientes

Parámetros (Tabla 1 del enunciado)
Tipo de acciónTipo de usuarioServ. (min)Llegada (min)ProbabilidadRetiroRápido110,23RetiroNormal220,40RetiroLento330,17RetiroMuy lento430,20PagoRápido310,10PagoNormal320,20PagoLento530,30PagoMuy lento740,40

70 % de los clientes realiza retiros · 30 % pagos o consignaciones

🚀 Cómo ejecutar
En Google Colab (recomendado)

Abrir el archivo Problema_Bancario.ipynb directamente en Google Colab https://colab.research.google.com/drive/1kXiRrVSVu4dUKzqKA6fJUB-8y3Ruwd9f?usp=sharing
Ejecutar la celda única con Ctrl + Enter o el botón ▶
Los resultados, gráficas y CSVs se generan automáticamente

Requiere: Python 3.8+, numpy, pandas, matplotlib, scipy


📊 Resultados principales
Punto 1 — Cajero con menor y mayor tiempo de atención
CajeroTiempo promedio de servicioCajero 13,826 min ❌ mayorCajero 22,990 min ✅ menorCajero 33,034 min
Punto 2 — Usuarios más y menos frecuentes

Más frecuente: Retiro Normal → 53,3 clientes/réplica
Menos frecuente: Pago Rápido → 6,3 clientes/réplica

Punto 3 — Réplica con menor afluencia

Réplica 2 con 181 clientes (promedio general: 198,5 · desv. est.: 11,2)

Punto 4 — ¿Se necesita un 4.º cajero?
Métrica3 cajeros4 cajerosReducciónEspera promedio0,399 min0,137 min65,8 %% espera > 5 min1,4 %0,6 %57,1 %Factor utilización (ρ)0,38——
Conclusión: No se recomienda. El sistema ya opera con un 62 % de capacidad ociosa.
Punto 5 — Configuración óptima
EscenarioEspera prom.Sistema prom.% > 5 min🏆 Mixto 3 cajeros0,399 min3,729 min1,4 %1R + 2P2,693 min5,859 min16,7 %2R + 1P2,254 min5,484 min11,3 %
La configuración mixta gana en todos los indicadores. La especialización genera cuellos de botella porque un cajero libre de un segmento no puede absorber la demanda del otro.

🧠 Aspectos técnicos destacados

Simulación de eventos discretos implementada desde cero con heapq de Python
Cola de prioridad no apropiativa: el tipo de usuario determina el orden de atención
Semillas reproducibles: semilla = i * 42 + 7 garantiza independencia entre réplicas
Métricas teóricas M/M/1 calculadas y contrastadas con los resultados empíricos (λ = 0,5073 /min · μ = 0,4408 /min · ρ = 0,38)
Intervalos de confianza al 95% con t-Student (gl = 9): espera ∈ [0,213 ; 0,572] min


📈 Gráficas generadas
ArchivoContenidoresultados_simulacion_banco.png6 paneles: espera por cajero, comparación de escenarios, distribución de tipos de usuario, espera por réplica, comparación 3 vs 4 cajeros, histograma de esperasanalisis_adicional_banco.pngClientes atendidos por réplica (retiros vs pagos) y tiempo de servicio promedio por tipo de usuario

📌 Conclusión general

El Banco de Colombia debe mantener sus tres cajeros en modalidad mixta. Esta configuración minimiza los tiempos de espera (< 0,4 min en promedio), garantiza que solo el 1,4 % de los clientes espere más de 5 minutos, y aprovecha de forma óptima la capacidad instalada sin necesidad de personal adicional.

