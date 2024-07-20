---
title: Objetos Marketo
feature: SOAP
description: Información general sobre objetos Marketo
exl-id: 99b9aed4-94e8-46e8-84d9-2cc5215b0c13
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# Objetos Marketo

Marketo utiliza objetos de Marketo (MObjects) para representar varias clases como Program, Opportunity y OpportunityPersonRole.

## Estructura de los objetos MO

Los objetos MO pueden ser de uno de los tres tipos siguientes: Estándar, Personalizado o Virtual. Los objetos MO estándar y personalizados representan entidades distintas, como Posible cliente o Empresa, mientras que los objetos virtuales, como LeadRecord, están compuestos por campos de uno o más objetos. Los objetos virtuales son objetos de conveniencia utilizados dentro de la API, pero que no existen dentro de la aplicación de Marketo.

Los objetos MO constan de:

- Un pequeño conjunto de atributos fijos que son comunes a todos los objetos MO
   - Tipo requerido
   - ExternalKey opcional
   - id de solo lectura, createdAt, updatedAt
- Una lista de uno o más atributos específicos de objeto, como pares de nombre/valor, algunos de los cuales pueden ser obligatorios. Por ejemplo, nombre en Oportunidad.
- Una lista de referencias de objeto asociadas, como object-name plus
   - MARKETO ID o
   - Clave externa como par atributo-nombre/atributo-valor.

### External-Keys

Las claves externas son campos personalizados definidos en objetos de Marketo, como cliente potencial u oportunidad. El nombre es el nombre del campo y el valor es el valor del campo, generado en un sistema externo. **Marketo no aplica una restricción única a estos valores.** Es responsabilidad del usuario de la API asegurarse de que los valores sean únicos. Si se produce un duplicado, Marketo utiliza el objeto añadido más recientemente. Esto es similar al comportamiento para el campo estándar Dirección de correo electrónico.

### API disponibles

| API | Puede operar en |
|---|---|
| describeMObject | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
| getMObjects | Oportunidad, OportunidadFunciónPersona, Programa |
| syncMObjects | Oportunidad, OportunidadFunciónPersona, Programa |
| deleteMObjects | Oportunidad, OportunidadFunciónPersona |
| listMObjects | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
