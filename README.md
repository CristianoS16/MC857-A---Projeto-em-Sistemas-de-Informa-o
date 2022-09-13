# Exemplo Simples de Uso no HL7 FHIR 

## Setup
Para facilitar a escrita dos arquivos JSON que representam os recursos FHIR a documentação disponibiliza um esquema: [Downloads - Schemas, Code, Tools](https://www.hl7.org/fhir/downloads.html).

Após fazer download do `JSON Schema` é necessária uma pequena configuração no VS Code.

```
{
  "json.schemas": [
    {
      "fileMatch": [
        "*.fhir.json"
      ],
      "url": "./fhir.schema.json"
    }
  ]
}
```

## Recursos FHIR

Veja a documentação dos resursos, [Resources](http://hl7.org/fhir/resourcelist.html), disponibilizados pelo HL7 FHIR.

Para melhor entender a documentação consulte [Resource Definitions](https://www.hl7.org/fhir/formats.html#table).

### Exemplo de Pacient

```
{
  "resourceType": "Patient",
  "name": [
    {
      "use": "official",
      "given": ["Brontossauro"],
      "family": "Brontosaurus"
    }
  ],
  "gender": "male",
  "birthDate": "2022-09-13",
  "telecom": [
    {
      "value": "40028922",
      "use": "mobile",
      "system": "phone"
    },
    {
      "system": "email",
      "value": "bronto@dac.unicamp.br"
    }
  ],
  "address": [
    {
      "line": [
        "1251, Av. Albert Einstein"
      ],
      "city": "Campinas",
      "state": "São Paulo",
      "postalCode": "13083-852"
    }
  ]
}
```

### Exemplo de Observation

```
{
  "resourceType": "Observation",
  "status": "final",
  "code": {
    "coding": [
      {
        "system": "http://loinc.org",
        "code": "15074-8",
        "display": "Glucose [Moles/volume] in Blood"
      }
    ]
  },
  "subject": {
    "reference": "Patient/PATIENT_ID",
    "display": "Brontossauro Brontosaurus"
  },
  "valueQuantity": {
    "value": 4.2,
    "unit": "mmol/1",
    "system": "http://unitsofmeasure.org.",
    "code": "mmol/L"
  }
}
```

Alguns exemplos de estrutura podem ser encontrados em [Rik Smithies/NProgram test server](http://nprogram.azurewebsites.net/).

Para muitos exemplos consulte a página [Downloads](https://www.hl7.org/fhir/downloads.html) do HL7 FHIR.

## Servers para Testes Publicos

Existem vários servidores para testes disponíveis online: [Publicly Available FHIR Servers for testing](https://wiki.hl7.org/Publicly_Available_FHIR_Servers_for_testing).

### HAPI FHIR Test/Demo Server R4 Endpoint

Pela simplicidade de acesso usei o [HAPI FHIR Test/Demo Server R4](https://hapi.fhir.org/baseR4/swagger-ui/).

_Atenção a consistência dos dado._

## Referências

[The Definitive FHIR Playlist](https://www.youtube.com/watch?v=HdPyV6ggGA4&list=PLUr-PTsPYKV4SHhszovqJ3v4hEhRezqzo&index=1)