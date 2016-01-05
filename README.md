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


# Work done on the credentials (set up with a symmetric key)
    - encryption of thepassw0rd:
                - open a Command Prompt
                - spring encrypt thepassw0rd --key maclestephanoise
                - copy the result into application.yml
    - test the decryption of thepassw0rd:
                - spring decrypt --key maclestephanoise e3ef56cf3ec969fb9f920b94f4ac3389a300297226b2985917637f3beb801953
                --> thepassw0rd
    - test with: curl -v -X GET http://localhost:8888/cspuserapi/dev/master --user user:thepassw0rd


# TODO Work done on the credentials (set up with an asymmetric key)
    - Read first http://projects.spring.io/spring-cloud/spring-cloud.html#_encryption_and_decryption_2

    - encryption of thepassw0rd:
            - open a Command Prompt
            - spring encrypt thepassw0rd --key @C:\Users\PBrossier\.ssh\mobileBackEnd.pem
            - copy the result into application.yml

    - create an empty keystore:
            - open a command prompt
            - keytool -genkey -alias foo -keystore "C:\data\perso\mytestkeystore.jks"
                    - password chosen = t1t1henr1
                    - CN=Philippe Brossier, OU=MyBiz, O=MyBiz, L=Southampton, ST=Hampshire, C=UK
            - keytool -delete -alias foo -keystore "C:\data\perso\mytestkeystore.jks"

    - add mobileBackEnd.pem to the keystore:
            - open a command prompt to C:\data\perso
            - openssl x509 -outform der -in mobileBackEnd.pem -out mobileBackEnd.der
            - keytool -import -alias your-alias -keystore cacerts -file mobileBackEnd.der

    - TODO: configure the keystore correctly --> see http://projects.spring.io/spring-cloud/spring-cloud.html#_key_management


# TODO decrypt props inbound.secured.endpoints.password
