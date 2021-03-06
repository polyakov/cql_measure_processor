# cql_measure_processor
CQL Measure Processing Component

## Usage 
  - $ mvn install
  - $ mvn -Djetty.http.port=XXXX jetty:run
  - HAPI Fhir server endpoint: http://localhost:XXXX/cql-measure-processor/baseDstu3
  - Tester page: http://localhost:XXXX/cql-measure-processor/tester/
  - This server specializes in Measure processing. To access this functionality you will need to follow the following steps:
    - Load all the resources you'll need to run the measure into the HAPI Fhir server
      - For example, if you wanted to load a condition with an ID of 1058 (JSON or XML formatted), use Postman (or other REST client) with the following request:
        - PUT http://localhost:XXXX/cql-measure-processor/baseDstu3/Condition/1058
        - OR POST http://localhost:XXXX/cql-measure-processor/baseDstu3/Condition if ID isn't specified (one will be provided at creation)
      - You may also load the Terminology information (CodeSystems and ValueSets) and use this as a Terminology Service (makes measure processing much more efficient). You may also use an external service by changing the JpaFhirTerminologyProvider to a FhirTerminologyProvider (https://github.com/DBCG/cql_engine/blob/master/cql-engine-fhir/src/main/java/org/opencds/cqf/cql/terminology/fhir/FhirTerminologyProvider.java) in the MeasureResourceProvider and then providing the endpoint to the service you would like to use.
        - A valueset loader is provided in this project, but it will run into problems if the valuesets you wish to load have import statements. We are currently working on a fix for this.
      - Load the Measure and Elm Library XML into the src\main\resources following the naming convention (i.e. MeasureID = col, Measure XML = col.xml, Measure Library = col.elm.xml)
      - After everything is loaded into the database you may run the measure like so:
        - GET http://localhost:XXXX/cql-measure-processor/baseDstu3/Measure/col/$evaluate?patient=Patient-12214&startPeriod=2014-01&endPeriod=2014-12
          - Format: Base/Measure/MeasureID/$evaluate?patient=PatientID&startperiod=Start date of Measure&endPeriod=End date of Measure
