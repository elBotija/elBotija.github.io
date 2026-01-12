---
layout: post
title: "AWS: Cómo ahorrar dinero en tu factura de AWS"
description: "Guía práctica para configurar AWS Budgets, usar Cost Explorer y apagar recursos ociosos para evitar sorpresas en la tarjeta de crédito."
tags: [AWS, Costos, FinOps, Tips, Tutorial]
---

El mayor miedo de cualquier desarrollador que empieza con AWS no es configurar mal una VPC o que se caiga el sitio. El verdadero terror es despertarse un día con una factura de 5.000 dólares porque dejaste una instancia EC2 `x1.32xlarge` prendida el fin de semana.

La nube es "pay-as-you-go" (pagás por lo que usás), pero si no tenés cuidado, podés terminar pagando por lo que **no** usás.

Acá te dejo 3 consejos prácticos para dormir tranquilo.

## 1. AWS Budgets: Tu alarma anti-infartos

Lo primero que tenés que hacer, **literalmente hoy**, es configurar un presupuesto.

1.  En la consola, buscá **"AWS Budgets"**.
2.  Hacé clic en **"Create budget"**.
3.  Elegí **"Cost budget"** (el default).
4.  Poné un monto mensual que estés dispuesto a pagar (por ejemplo, **$10 USD**).
5.  Configurá una alerta para que te mande un mail cuando llegues al **80%** de ese presupuesto.

Listo. Si algo se sale de control, te enterás antes de que sea tarde.

## 2. Cost Explorer: ¿En qué se me va la plata?

A veces la factura sube y no sabés por qué. **Cost Explorer** es tu amigo.

Entrá a la consola de **Billing and Cost Management** y buscá **Cost Explorer** a la izquierda.

*   Filtrá por **"Service"** para ver qué servicio está gastando más (¿es EC2? ¿es RDS?).
*   Agrupá por **"Usage Type"** para ver el detalle (¿es transferencia de datos? ¿es almacenamiento?).

Entender tus costos es el primer paso para bajarlos.

## 3. Limpieza de Primavera (Recursos Zombies)

Es muy común crear recursos para probar algo y olvidarse de borrarlos. Los culpables más comunes son:

*   **Volúmenes EBS Huérfanos:** Cuando borrás una instancia EC2, a veces el disco (EBS) queda vivo y te lo siguen cobrando. Buscá volúmenes con estado `available` (no `in-use`) y borralos.
*   **Elastic IPs:** AWS te cobra por las IPs estáticas que **no** están asociadas a una instancia en ejecución. Si tenés una IP reservada y no la usás, estás tirando plata.
*   **NAT Gateways:** Son carísimos. Si tenés una VPC de prueba, asegurate de borrar los NAT Gateways cuando termines.
*   **Snapshots viejos:** Los backups de RDS o EBS se acumulan. Configurá políticas de ciclo de vida o borrá los viejos manualmente.

## Conclusión

No necesitás ser un experto en FinOps para cuidar tu bolsillo. Con un presupuesto básico y una revisión mensual de recursos zombies, podés experimentar en la nube sin miedo.

En el próximo artículo vamos a ver cómo interactuar con AWS usando Python y la librería `boto3`. ¡Hasta la próxima!
