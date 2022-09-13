# Equipe Datassauros

| Nome                       | RA     |
| -------------------------- | ------ |
| Artur Abreu                | 231713 |
| Cristiano Sampaio Pinheiro | 256352 |
| George Gigilas Junior      | 216741 |
| Jhonatan Cléto             | 256444 |
| Mylena Roberta dos Santos  | 222687 |

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


## APIs FHIR em Python

### fhir-py

O [fhir-py](https://github.com/beda-software/fhir-py) é um cliente FHIR async/sync em Python3 que prove uma API para realizar operações CRUD sobre os recurcos do FHIR.

#### Exemplo do fhir-py

~~~Python
import asyncio
from fhirpy import AsyncFHIRClient


async def main():
    # Create an instance
    client = AsyncFHIRClient(
        'http://fhir-server/',
        authorization='Bearer TOKEN',
    )

    # Search for patients
    resources = client.resources('Patient')  # Return lazy search set
    resources = resources.search(name='John').limit(10).sort('name')
    patients = await resources.fetch()  # Returns list of AsyncFHIRResource

    # Create Organization resource
    organization = client.resource(
        'Organization',
        name='beda.software',
        active=False
    )
    await organization.save()

    # Update (PATCH) organization. Resource support accessing its elements
    # both as attribute and as a dictionary keys
    if organization['active'] is False:
        organization.active = True
    await organization.save(fields=['active'])
    # `await organization.update(active=True)` would do the same PATCH operation

    # Get patient resource by reference and delete
    patient_ref = client.reference('Patient', 'new_patient')
    # Get resource from this reference
    # (throw ResourceNotFound if no resource was found)
    patient_res = await patient_ref.to_resource()
    await patient_res.delete()

    # Iterate over search set
    org_resources = client.resources('Organization')
    # Lazy loading resources page by page with page count = 100
    async for org_resource in org_resources.limit(100):
        print(org_resource.serialize())


if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())
~~~

#### Learn basics of FHIR with Jupyter

[Repositório](https://github.com/Aidbox/jupyter-course) com um curso com cinco exercícios (Jupyter notebooks) ensinando a como consumir os recursos do FHIR. Utiliza o fhir-py consumindo a API [Health Samurai](https://www.health-samurai.io/).

### SMART on FHIR

[Cliente em python](https://github.com/smart-on-fhir/client-py) para servidores FHIR que suportam o protocolo [SMART on FHIR](http://docs.smarthealthit.org/). 

#### Exemplo

~~~Python
from fhirclient import client
settings = {
    'app_id': 'my_web_app',
    'api_base': 'https://fhir-open-api-dstu2.smarthealthit.org'
}
smart = client.FHIRClient(settings=settings)

import fhirclient.models.patient as p
patient = p.Patient.read('hca-pat-1', smart.server)
patient.birthDate.isostring
# '1963-06-12'
smart.human_name(patient.name[0])
# 'Christy Ebert'
~~~

## Referências

[The Definitive FHIR Playlist](https://www.youtube.com/watch?v=HdPyV6ggGA4&list=PLUr-PTsPYKV4SHhszovqJ3v4hEhRezqzo&index=1)