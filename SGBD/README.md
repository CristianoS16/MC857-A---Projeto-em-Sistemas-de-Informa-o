# Equipe Datassauros

| Nome                       | RA     |
| -------------------------- | ------ |
| Artur Abreu                | 231713 |
| Cristiano Sampaio Pinheiro | 256352 |
| George Gigilas Junior      | 216741 |
| Jhonatan Cléto             | 256444 |
| Mylena Roberta dos Santos  | 222687 |

# Proposta de SGBD

## Modelagem dos Banco de Dados

Modelo de um paciente gerado pelo [Synthea](https://github.com/synthetichealth/synthea/wiki).

### Patients
| | Column Name | Data Type | Required? | Description |
|-|-------------|-----------|-----------|-------------|
| :key: | Id | UUID | `true` | Primary Key. Unique Identifier of the patient. |
| | BirthDate | Date (`YYYY-MM-DD`) | `true` | The date the patient was born. |
| | DeathDate | Date (`YYYY-MM-DD`) | `false` | The date the patient died. |
| | ~~SSN~~ | String | `true` | Patient Social Security identifier. |
| | ~~Drivers~~ | String | `false` | Patient Drivers License identifier. |
| | ~~Passport~~ | String | `false` | Patient Passport identifier. |
| | ~~Prefix~~ | String | `false` | Name prefix, such as `Mr.`, `Mrs.`, `Dr.`, etc. |
| | First | String | `true` | First name of the patient. |
| | Last | String | `true` | Last or surname of the patient. |
| | ~~Suffix~~ | String | `false` | Name suffix, such as `PhD`, `MD`, `JD`, etc. |
| | Maiden | String | `false` | Maiden name of the patient. |
| | Marital | String | `false` | Marital Status. `M` is married, `S` is single. Currently no support for divorce (`D`) or widowing (`W`) |
| | Race | String | `true` | Description of the patient's primary race. |
| | Ethnicity | String | `true` | Description of the patient's primary ethnicity. |
| | Gender | String | `true` | Gender. `M` is male, `F` is female. |
| | BirthPlace | String | `true` | Name of the town where the patient was born. |
| | Address | String | `true` | Patient's street address without commas or newlines. |
| | City | String | `true` | Patient's address city. |
| | State | String | `true` | Patient's address state. |
| | County | String | `false` | Patient's address county. |
| | Zip | String | `false` | Patient's zip code. |
| | ~~Lat~~ | Numeric | `false` | Latitude of Patient's address. |
| | ~~Lon~~ | Numeric | `false` | Longitude of Patient's address. |
| | ~~Healthcare_Expenses~~ | Numeric | `true` | The total lifetime cost of healthcare to the patient (i.e. what the patient paid). |
| | ~~Healthcare_Coverage~~ | Numeric | `true` | The total lifetime cost of healthcare services that were covered by Payers (i.e. what the insurance company paid). |
| | ~~Income~~ | Numeric | `true` | Annual income for the Patient |


Synthea apresenta resultados de exames como observações, não aparenta ser otimizado para esse projeto.

### Observations
| | Column Name | Data Type | Required? | Description |
|-|-------------|-----------|-----------|-------------|
| | Date | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | `true` | The date and time the observation was performed. |
| :old_key: | Patient | UUID | `true` | Foreign key to the Patient. |
| :old_key: | ~~Encounter~~ | UUID | `true` | Foreign key to the Encounter where the observation was performed. |
| | Category | String | `false` | Observation category. |
| | Code | String | `true` | Observation or Lab code from LOINC |
| | Description | String | `true` | Description of the observation or lab. |
| | Value | String | `true` | The recorded value of the observation. |
| | Units | String | `false` | The units of measure for the value. |
| | Type | String | `true` | The datatype of `Value`: `text` or `numeric` |

### Principais dados em um hemograma

<table> <thead><tr tablehead1=""><th style="text-align:left" scope="col"><p class="tableHead1"><span id="v55338858" class="anchor"></span></p><div class="para"> <p>Test</p></div><p></p></th><th style="text-align:left" scope="col"><p class="tableHead1"><span id="v55338861" class="anchor"></span></p><div class="para"> <p>What It Measures</p></div><p></p></th><th style="text-align:left" scope="col"><p class="tableHead1"><span id="v55338864" class="anchor"></span></p><div class="para"> <p>Normal Values</p></div><p></p></th></tr></thead> <tbody><tr class="odd"><td><span id="v55338872" class="anchor"></span><div class="para"> <p>Hemoglobin </p></div></td><td><span id="v55338874" class="anchor"></span><div class="para"> <p>Amount of this oxygen-carrying protein within a volume of blood </p></div></td><td><span id="v55338876" class="anchor"></span><div class="para"> <p>Men: 14 to 17 grams per deciliter (140 to 170 grams/L)</p></div><span id="v55338877" class="anchor"></span><div class="para"> <p>Women: 12 to 16 grams per deciliter (120 to 160 grams/L)</p></div></td></tr><tr class="even"><td><span id="v55338880" class="anchor"></span><div class="para"> <p>Hematocrit </p></div></td><td><span id="v55338882" class="anchor"></span><div class="para"> <p>Proportion of the total amount of blood (blood volume) made up of red blood cells (plasma makes up the rest) </p></div></td><td><span id="v55338884" class="anchor"></span><div class="para"> <p>Men: 41 to 51%</p></div><span id="v55338885" class="anchor"></span><div class="para"> <p>Women: 36 to 47%</p></div></td></tr><tr class="odd"><td><span id="v55338888" class="anchor"></span><div class="para"> <p>Mean cellular (or corpuscular) volume (MCV) </p></div></td><td><span id="v55338890" class="anchor"></span><div class="para"> <p>Average volume of a red blood cell </p></div></td><td><span id="v55338892" class="anchor"></span><div class="para"> <p>80 to 100 femtoliters per cell</p></div></td></tr><tr class="even"><td><span id="v55338895" class="anchor"></span><div class="para"> <p>Mean cellular (or corpuscular) hemoglobin (MCH)</p></div></td><td><span id="v55338897" class="anchor"></span><div class="para"> <p>Amount of hemoglobin per red blood cell</p></div></td><td><span id="v55338899" class="anchor"></span><div class="para"> <p>28 to 32 picograms per cell</p></div></td></tr><tr class="odd"><td><span id="v55338902" class="anchor"></span><div class="para"> <p>Mean cellular (or corpuscular) hemoglobin concentration (MCHC) </p></div></td><td><span id="v55338904" class="anchor"></span><div class="para"> <p>Average concentration of hemoglobin within red blood cells </p></div></td><td><span id="v55338906" class="anchor"></span><div class="para"> <p>32 to 36 grams per deciliter of red blood cells (320 to 360 grams per liter) </p></div></td></tr><tr class="even"><td><span id="v55338909" class="anchor"></span><div class="para"> <p>Red blood cell (RBC) count</p></div></td><td><span id="v55338911" class="anchor"></span><div class="para"> <p>Number of red blood cells in a volume of blood</p></div></td><td><span id="v55338913" class="anchor"></span><div class="para"> <p>Men: 4.5 to 5.9 million cells per microliter (4.5 to 5.9 × 10<sup>12</sup>/L)</p></div><span id="v55338914" class="anchor"></span><div class="para"> <p>Women: 4.0 to 5.2 million cells per microliter (4.05 to 5.2 × 10<sup>12</sup>/L)</p></div></td></tr><tr class="odd"><td><span id="v55338917" class="anchor"></span><div class="para"> <p>Red cell distribution width</p></div></td><td><span id="v55338919" class="anchor"></span><div class="para"> <p>Amount of variability in the sizes of the red blood cells</p></div></td><td><span id="v55338921" class="anchor"></span><div class="para"> <p>11.5 to 14.5%</p></div></td></tr><tr class="even"><td><span id="v55338924" class="anchor"></span><div class="para"> <p>White blood cell count </p></div></td><td><span id="v55338926" class="anchor"></span><div class="para"> <p>Number of white blood cells in a specified volume of blood </p></div></td><td><span id="v55338928" class="anchor"></span><div class="para"> <p> 4,500 to 11,000 per microliter (4.5 to 11 × 10<sup>9</sup>/L) </p></div></td></tr><tr class="odd"><td><span id="v55338931" class="anchor"></span><div class="para"> <p>Differential white blood cell count </p></div></td><td><span id="v55338933" class="anchor"></span><div class="para"> <p>Percentages and numbers of the different types of white blood cells </p></div></td><td><span id="v55338935" class="anchor"></span><div class="para"> <p>Segmented neutrophils: 40 to 70%, or 1800 to 7700 per microliter (1.8 to 7.7 x 10<sup>9</sup>/L)</p></div><span id="v55338936" class="anchor"></span><div class="para"> <p>Lymphocytes: 22 to 44%, or 1000 to 4800 per microliter (1 to 4.8 × 10<sup>9</sup>/L)</p></div><span id="v55338937" class="anchor"></span><div class="para"> <p>Monocytes: 4 to 11%, or 200 to 1200 per microliter (0.2 to 1.2 × 10<sup>9</sup>/L)</p></div><span id="v55338938" class="anchor"></span><div class="para"> <p>Eosinophils: 0 to 8%, or 0 to 900 per microliter (0 to 0.9 × 10<sup>9</sup>/L)</p></div><span id="v55338939" class="anchor"></span><div class="para"> <p>Basophils: 0 to 3%, or 0 to 300 per microliter (0 to 0.3 × 10<sup>9</sup>/L)</p></div></td></tr><tr class="even"><td><span id="v55338942" class="anchor"></span><div class="para"> <p>Platelet count </p></div></td><td><span id="v55338944" class="anchor"></span><div class="para"> <p>Number of platelets in a specified volume of blood </p></div></td><td><span id="v55338946" class="anchor"></span><div class="para"> <p>140,000 to 450,000 per microliter (140 to 450 × 10<sup>9</sup>/L)</p></div></td></tr></tbody> <tfoot><tr><td colspan="3"><span id="v55338868" class="anchor"></span><div class="para"> <p>* Normal values vary from laboratory to laboratory.</p></div></td></tr></tfoot> </table>

[Complete Blood Count - MSD Manual](https://www.msdmanuals.com/en-kr/home/multimedia/table/complete-blood-count-cbc)

Pode-se implementar uma base contendo todos os dados do exame acrescidos de uma chave estrangeira para o paciente e um campo contendo a data do exame.
As bases usadas devem esta de acordo com o os padões do OpenEHR e possibilitar a geração de dados através de algum software.

## OpenEHR: Modelo e Armazenamento de Dados

É necessário reunir especialistas na área de saúde para modelar e configurar a implementação do openEHR de acordo com as necessidades do sistema. Em todo caso, o modelo vai ser construído em cima do modelo de referência do openEHR, que permite uma grande robustez do sistema, principalmente para o armazenamento dos dados. Além de todo esse ferramental, o openEHR também possui uma query language (similar a um SQL), que otimiza a busca por informações nesse banco de dados.

Uma vez construído, o modelo será bastante poderoso. Será então necessário facilitar sua comunicação com os outros componentes do sistema, o que poderá ser feito através do FHIR. Mais detalhes sobre isso se encontram na próxima seção.

[Patient Archetype OpenEHR](https://ckm.openehr.org/ckm/archetypes/1013.1.821/mindmap)

![Patient Archetype](assets/openehr-patient.png)

[EHRbase](https://ehrbase.org/)

https://stackoverflow.com/questions/25449018/how-is-openehr-supposed-to-be-used

## Comunicação entre OpenEHR DB e HL7 FHIR

Um dos maiores desafios da integração e que ainda não há soluções prontas para fazer a transcrição entre OpenEHR e FHIR. O software deve ser desenvolvido sobre medida ou desenvolver um mecanismo de integração proprietário.

Diversos autores e usuários dessas tecnologias afirmam que é muito vantajoso integrar os dois. Enquanto o openEHR é um sistema poderoso para armazenar os dados de forma consistente e permanente, o FHIR é ótimo para transferência de dados e uso em APIs. Seguem algumas formas encontradas para uso integrado dos dois:

![FHIR and openEHR integration](assets/FHIR+openEHR.png)

Pelo que foi pesquisado, são 4 as formas mais utilizadas:
+ Facade: utilizar um banco de dados segundo o padrão openEHR e realizar operações sobre ele com o FHIR e com próprio openEHR. O artifício que permite o uso do FHIR é um componente de tradução entre os dois padrões. Essa forma é interessante pois concentra tudo em um único banco de dados, combinando as vantagens de cada padrão.
+ Message Broker: uma ideia que funciona melhor na teoria do que na prática consiste em utilizar message brokers (ou tradutores de padrões) para possibilitar que os padrões se comuniquem. Porém, não parece muito vantajoso ter 2 bancos de dados e com modelos adotados diferentes.
+ Sync Agent: consiste em ter dois bancos de dados, um para cada modelo, e utilizar um "agente sincronizador" para manter os dois atualizados. Novamente, não parece interessante guardar a mesma informação em dois bancos de dados diferentes.
+ Sharded: essa implementação pode ser interessante, pois consiste em ter um banco de dados para cada padrão, de acordo com o padrão mais adequado para cada tipo de dado. Por exemplo, pode-se utilizar o openEHR para guardar dados de exames dos pacientes e o FHIR para guardar dados administrativos

[FHIR + openEHR — 2022](https://medium.com/@alastairallen/fhir-openehr-2022-53716f837340)

[Mapping of OpenEHR Archetypes to
FHIR Resources in Use Case Oncology](https://ebooks.iospress.nl/pdf/doi/10.3233/SHTI210616)
