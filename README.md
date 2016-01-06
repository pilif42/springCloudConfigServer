# springCloudConfigServer
Server providing support for externalized configuration in a distributed system


# To build
gradlew clean build


# To access props (credentials specified in application.yml: see below for details - url in /{application}/{profile}[/{label})
curl -v -X GET http://localhost:8888/restfulwebservice/local/master --user user:thepassw0rd
200 {"name":"restfulwebservice","profiles":["local"],"label":"master","propertySourc
    es":[{"name":"https://github.com/pilif42/springCloudConfiguration/restfulwebserv
    ice-local.properties","source":{"logging.level.com.example.springboot":"DEBUG","
    logging.level.org.springframework.cloud":"DEBUG","logging.file":"C:/data/mytests
    /springboot_ws.txt","spring.profiles.active":"local","logging.level.org.hibernat
    e":"ERROR","datasource.url":"jdbc:postgresql://localhost:5432/teststore","elasti
    csearch.server.name":"127.0.0.1","logging.level.org.springframework.web":"DEBUG"
    }},{"name":"https://github.com/pilif42/springCloudConfiguration/restfulwebservic
    e.properties","source":{"server.port":"8089","datasource.username":"teststore","
    inbound.secured.endpoints.username":"user","elasticsearch.server.port":"9300","d
    atasource.password":"teststore","inbound.secured.endpoints.password":"inboundpas
    sword"}},{"name":"https://github.com/pilif42/springCloudConfiguration/applicatio
    n.properties","source":{"spring.jpa.show-sql":"true","spring.jpa.hibernate.namin
    g_strategy":"org.hibernate.cfg.ImprovedNamingStrategy","spring.jpa.database":"po
    stgresql","spring.jpa.hibernate.ddl-auto":"none","spring.jpa.database-platform":
    "org.hibernate.dialect.PostgreSQLDialect"}}]}


# Work done on the credentials (set up with a symmetric key)
    - encryption of thepassw0rd:
                - open a Command Prompt
                - spring encrypt thepassw0rd --key maclestephanoise
                - copy the result into application.yml
    - test the decryption of thepassw0rd:
                - spring decrypt --key maclestephanoise e3ef56cf3ec969fb9f920b94f4ac3389a300297226b2985917637f3beb801953
                --> thepassw0rd
    - test with: curl -v -X GET http://localhost:8888/restfulwebservice/local/master --user user:thepassw0rd


# Work done on the credentials (set up with an asymmetric key - more secure)
    - Read first http://projects.spring.io/spring-cloud/spring-cloud.html#_encryption_and_decryption_2
            - we will need a public key to encrypt props.
            - we will need a private key (ideally stored in the config server) to decrypt props and serve them to clients.
    - created a public key and a private key:
            - Git bash to c/data/mytests
            - ssh-keygen -t rsa -b 4096 -C "brossierp@gmail.com"
            - saved as springcloudserver_rsa and springcloudserver_rsa.pub with passphrase = t1t1henr1
    - encryption of thepassw0rd with springcloudserver_rsa.pub:
            - open a Command Prompt
            - spring encrypt thepassw0rd --key @C:\data\mytests\springcloudserver_rsa.pub
            - copy the result into application.yml
    - convert private rsa key into PEM format:
            - command prompt to C:\data\mytests
            - openssl rsa -in springcloudserver_rsa -outform PEM -out springcloudserver_rsa.pem
    - gave encrypt.key the value of the private PEM key
    - started the server but fails with exception due to 'Non-hex character in' the encrypted value of thepassw0rd
    - TODO: restart here



