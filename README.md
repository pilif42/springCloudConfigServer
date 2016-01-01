# springCloudConfigServer
Server providing support for externalized configuration in a distributed system


# To build
gradlew clean build


# To access props (credentials specified in application.yml - see below for details)
curl -v -X GET http://localhost:8888/cspuserapi/dev/master --user user:thepassw0rd
200 {"name":"cspuserapi","profiles":["dev"],"label":"master","propertySources":[{"na
    me":"https://github.com/pilif42/springCloudConfiguration/application.properties"
    ,"source":{"inbound.secured.endpoints.username":"user","inbound.secured.endpoint
    s.password":"AQBMhGXzlfEHcaumM/TwF0G9QCKuNGXl4eQ7ibqODNRHjrr1fkYT3KrBmqRFBbv8LC6
    W0zYI1fguYb2eytBWCuERrA74n0lb621lz/PeClzk9yhjnwxKFGKM5YpOumo6WjuyDygkcNic8gIo1Wy
    GE1lBuGqwzaW1r92zO9mLSlvOeDgTHWVZluQbjcwbuMdhk7EOVcz7aYams1Z22vMyS/0/E9K+8ijYz/v
    cmWB//rhGfki39jva+aRZBV8SWf3aGLmTOHYqZjsfB6g2tLh3Ruuqh1NJePUQTSSxITnTH+/d1JFNT7G
    Bp/wFjhLslmdVI0cAiK8uMjRhxTFshj3MRWk+0vqqj6I2ZdJtGE5hgkleye2j0ZEzBLBiGrae2qpFN8M
    ="}}]}


# Work done on the credentials
    - encryption of thepassw0rd:
            - open a Command Prompt
            - spring encrypt thepassw0rd --key @C:\Users\PBrossier\.ssh\mobileBackEnd.pem
            - copy the result into application.yml
    - TODO: configure the keystore correctly --> see http://projects.spring.io/spring-cloud/spring-cloud.html#_key_management


# TODO decrypt props inbound.secured.endpoints.password
