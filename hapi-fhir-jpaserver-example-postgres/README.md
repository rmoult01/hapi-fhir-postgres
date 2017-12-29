## FHIR Server using Postgresql database

### Setup

1. I installed PostgreSQL 9.5.10 using std port, 5432.
2. Create linux user hapi.
3. Add hapi user to postgres as admin with password "mysecretpassword".
4. Set up desired access method in pg_hba.conf. I just used trust:
```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             samehost                trust
```

### Read only server option.

This defaults to 'false', meaning the server functions normally.
To set it on, pass the environment variable SERVER_READ_ONLY to tomcat
with a value of '1' or 'true'. Reminder, this can be done by putting
it in the setenv.sh file in the tomcat bin directory.

### How to add servers to the test client dropdown list.

At the top of the test client window there is an entry "Server:", which
indicates to the test client which fhir server to send requests to. This
defaults to "Local Tester", which is this server. Additional servers can
be added to this list by editing the constructor in class
ca.uhn.fhir.jpa.demo.FhirTesterConfig in the

/hapi-fhir/hapi-fhir-jpaserver-example-postgres

module. For example:
```
public TesterConfig testerConfig() {
   TesterConfig retVal = new TesterConfig();
       retVal
          .addServer()
             .withId("home")
             .withFhirVersion(FhirVersionEnum.DSTU3)
             .withBaseUrl("${serverBase}/baseDstu3")
             .withName("Local Tester");
      // this is what is added
      retval
         .addServer()
            .withId("other")
            .withFhirVersion(FhirVersionEnum.DSTU3)
            .withBaseUrl("http://123.10.44.174:9230/baseDstu3")
            .withName("Other Server");
      //
      return retVal;
   }
```
Be sure **NOT** to use the ${serverBase} parameter, as that is the
base for our fhir server.


