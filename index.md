###  Proyecto final: Instalación y configuración de la pila ELK.

**Tabla de contenidos**   
1. [Definición del proyecto](#id1)
2. [Objetivos planteados](#id2)
3. [Instalación de ambientes](#id3)
    * [Windows](#id31)
        - [Hyper-V](#id311)
        - [IIS](#id312)
        - [Active Directory](#id313)
        - [DNS](#id314)
        - [DHCP](#id315)
        - [SQL Server](#id316)
    * [Linux](#id32)
        - [Correo electrónico](#id3201)
            * [SMTP, SMTPS](#id32011)
            * [POP3, POP3S, IMAP, IMAPS](#id32012)
            * [Amavis y Spamassassin](#id32013)
        - [LDAP](#id3202)
        - [LDAPS](#id3203)
        - [Apache HTTPD (HTTP,HTTPS)](#id3204)
        - [NginX (HTTP, HTTPS)](#id3205)
        - [SSH](#id3206)
        - [Firewall (Netfilter, fail2ban)](#id3207)
        - [WAF (modSecurity para Apache HTTPD y NginX)](#id3208)
        - [PostgreSQL](#id3209)
        - [MySQL/MariaDB](#id3210)
        - [ProFTPD](#id3211)
    * [Dispositivos de red](#id33)
4. [Instalación de BELK Stack](#id4)
    * [Beats](#id401)
        - [CentOS](#id4011)
        - [Debian](#id4012)
        - [Windows](#id4013)
        - [Dispositivos de red](#id4014)
    * [Elasticsearch](#id402)
    * [Logstash](#id403)
    * [Kibana](#id404)
    * [Configuración de TLS-HTTPS](#id405)
    * [Generación de visualizaciones de kibana](#id406)
    * [Dashboards](#id407)
        - [Email Dashboard](#id4071)
        - [Web Dashboard](#id4072)
        - [WAF Dashboard](#id4073)
        - [Iptables Dashboard](#id4074)
        - [FTP-DB Dashboard](#id4075)
        - [Windows Dashboard](#id4076)
    * [Alertas](#id408)
        - [Alertas de Email](#id4081)
        - [Alertas de WAF](#id4082)
        - [Alertas de Web](#id4083)
        - [Alertas de Iptables y fail2ban](#id4084)
        - [Alertas de base de datos](#id4085)
        - [Alertas de FTP](#id4086)
    * [Conectores](#id409)
    * [Reportes](#id410)
5. [Pentest](#id5)
    * [Resumen ejecutivo](#id51)
        - [Puntos positivos](#id511)
        - [Puntos negativos](#id512)
    * [Introducción](#id52)
        - [Objetivo](#id521)
        - [Restricciones](#id522)
        - [Alcance](#id523)
        - [Antecedentes](#id524)
        - [Herramientas usadas](#id525)
    * [Vulnerabilidades](#id53)
    * [Hallazgos](#id54)
    * [Recomendaciones](#id55)
6. [Conclusión](#id6)
7. [Bibliografía](#id7)

# Definición del proyecto.<a name="id1"></a>

El proyecto consiste en desarrollo de una infraestructura que implemente la pila BELK (Beats, Elasticsearch, Logstash, Kibaba) para que de esta forma tenga la capacidad de generar reportes, informes, tablas, gráficos e identificar patrones anómalos a través de la correlación de eventos.

Los elementos de la pila BELK son los siguientes:

-   Beats: Son agentes de datos de código abierto que se instalan en los servidores para enviar datos operativos a Elasticsearch. Beats puede enviar datos directamente a Elasticsearch o a través de Logstash.
    
-   Elasticsearch: Es un motor de análisis, búsqueda y almacenamiento distribuido en tiempo real.
    
-   Logstash: Es un motor de recopilación de datos de código abierto con capacidades de canalización en tiempo real. Logstash puede unificar dinámicamente datos de fuentes dispares y normalizar los datos en destinos de su elección.
    
-   Kibana: Es una plataforma de análisis y visualización de código abierto que permite la integración con Elasticsearch. Los principales usos de Kibana son las búsquedas, visualizaciones e interacciones con los datos almacenados en los índices de Elasticsearch, así como la posibilidad de generar una gran variedad de gráficos, tablas y mapas.
    

También se cuenta con una infraestructura de servicios en ambientes Windows y Linux que servirán como insumo para el análisis y procesamiento de bitácoras por medio de los servicios provistos por la pila BELK.

  

# Objetivos planteados.<a name="id2"></a>

El principal objetivo es el desarrollo de una infraestructura que implemente la pila BELK, para ello es necesario implementar una estructura con equipos que puedan ser monitoreados y que generan logs para ser recopilados por la pila BELK.

Otro objetivo que se tiene es la creación de un script que permita establecer la línea base de seguridad de los distintos equipos que serán monitoreados.

  
  
  
  
  

# Instalación de ambientes.<a name="id3"></a>

En esta sección de la documentación se encuentra registrado la instalación de los servicios separados de acuerdo al ambiente al cual pertenecen.

  
  
  

## Windows.<a name="id31"></a>

Para esta infraestructura se tomó la decisión de crear dos servidores con sistemas Windows Server 2019 para una mejor distribución del trabajo. Una de estas máquinas cuenta con los servicios de Hyper-V, IIS y Active Directory, el otro servidor Windows tiene los servicios de DNS, DHCP y SQLServer, ambos servidores son virtualizados en VMWare.

  

### Hyper-V.<a name="id311"></a>

Para realizar la instalación de Hyper-V en un servidor Windows Server 2019 hacemos uso del programa Server Manager en el cual seleccionamos la opción de “Add roles and features”.

  

![](https://lh6.googleusercontent.com/lfbPY17QLxgeTwnI2Al9W6FsxdWJz3LAFNciX9c4SjuutAQR348V-Ou268ppNbLKaLWttTCsP47gmNBX5hwyeMr0obxJmL2RMe8-xEXWirP2VSZ5zMonLS0aLFg1U2O8h5FFQyCU)

  
  
  

Se ejecuta un wizard el cual nos apoyara en la instalación, en la primera pantalla nos muestra una breve explicación de lo que puede hacer este asistente de instalación.

![](https://lh4.googleusercontent.com/LCKayoRr2p10GWDxMWUjDiQhVWeK1aRyremSeCFFMQRHsNG8KjWQ884TvnyKuF2slP9-Ki0Bym3vJfEqHqNkrP3YiUwJKfLSxGIoCXHeZRDkuR9UV5NE51L9eFC_dwU-0ebH_HAl)

Seleccionamos el tipo de instalación que para nuestro caso “Role-based or feature-based installation” y damos siguiente.

![](https://lh6.googleusercontent.com/mwihspAywczuSbTkxmgd_QT9BmEhfmPHU4cQn6JwtSjCG6oMlfk4NcjvHUw4ieYeWH34acP7ARmAGZKDabaGfgXE1hjdfGhsE4ryDHS6FqDRS1UwK-g9yNefnrUG-WS5wHNlmZxY)

Posteriormente debemos seleccionar en qué servidor deseamos instalar. Estas últimas tres pantallas se repiten siempre que se realiza una instalación de algún rol o característica de Windows por ello se omitirá su explicación en las siguientes instalaciones.

![](https://lh4.googleusercontent.com/Yf3scn8jc6Zug4XvW-E8Dy-Wm2Qs4J8jYQr2sABIKscGJXtDdDqNxmxhko0A12O96xkKtO0S5QokQ78LBVo_JMXiJ8jwV_58Lwrab0cLEF9Td4RnR6I6BOl7aA2dYmcAYKRzC13W)

Seleccionamos el rol que queremos instalar que en este caso concreto es Hyper-V.

![](https://lh3.googleusercontent.com/XEBCUwuQgujvBoy0w6vgPO78zWqdyGYvM14vSJiZCeEzm0cqR1Ld3pfxSjPKRPLWzFWKES4rKc03wORyC8v_dr4Sz2RZ-F-XQIjcevOoAhTXUv-pFnzYZPQ3JX4i5_tj3m7ekScB)

Una vez seleccionado nos muestra que características son las que se instalarán o para que el servicio funcione.

![](https://lh3.googleusercontent.com/euS0dXRgo7G0joN0WKY7f9vz_PYHL09U6qW_0SnL856WmKNi1JWBxT_XSK5c5cTgUOPdq3Y1q9XvF13Bh_X6jt7hkfP6GJDhd_mQYsMvImf-EFEH2au9Din3Of3xSynamliFOdVz)

  

Después nos muestra una pantalla para agregar características en el cual no se realiza nada. Posterior a esa pantalla nos mostrará una breve descripción de Hyper-V

![](https://lh3.googleusercontent.com/XQEx986majtSjr3BYIahxeAKFegApg6CQNDEgMZRRZ6m3tF2w6EbXkPalrDewzGEcy5c6a_R3IT6LcWhiqoHocrtIhh9k7aJ7DS-mkubtlUQ0ZvKZdobopypOXqdWhGEhI13xf4d)

  
  

Después creamos un switch virtual para que el futuro uso de las máquinas virtuales.

![](https://lh5.googleusercontent.com/mt0hC8dJrjZPbRXkRHBzvVpfVVpm4_O6z5oH2Vi0rjuCCaWCwcl3khGXHeDNfnisWN8IVb-vlpFpDHgh85xKIMMFmTaBPwwY1VCoSPcy1cQj8S5jFLzRXh3p-ir1oRxE3Z24Hjfg)

Después se debe configurar la migración de máquinas virtuales, para nuestro caso no es necesario así que no habilitamos tal opción.

![](https://lh6.googleusercontent.com/dxv5C81gp_2nYi4EjRDQMoYcYfB7VPyK9nEBOzuft5Kl6LWH-GO8dikgz5N-0XOYg1mDBa3Yz24RxlzVqX4fBtW0yCEFVfsv7HchyZZa7gxxgf6E6xVKplqQAHhrprug7rPK84Gq)

  

Posteriormente se configura las rutas en las cuales se guardarán las máquinas virtuales y los discos virtuales.

![](https://lh6.googleusercontent.com/Mjgn5vyh3tR1vK_ofttsq9mtsENHemM6Q1G7VKIIQwD4vK2E_X8IjgsxIJn8oBzGw56QZYJkCe1IyljzuHOeN_FuzC_gxRSCE4QdcQblSyJh5cMEQu8Ui5UpM2rMCWQjvZ9KsxAB)

Por último nos mostrará una pantalla donde se puede observar un resumen de lo que se instalará.

![](https://lh3.googleusercontent.com/fOIgEgpI66wGUZx6rZLwdpwTe3ND5ZQKq7c8Hiip10vO0dasygvDY1GwFZ7BQ2K0BmBGgd9iu7nSyKBq-PmRHvMI1xTxpuFqV_kOMcB3FJgqDQsNNu8FGu4jA5LbaIyhk0PnDhuf)

  

Una vez instalado es necesario reiniciar el equipo.

![](https://lh5.googleusercontent.com/JBKE4vTNoBW4zQ7oyYvGE2Amtd3rDFrC_QfKNVNf02KLF2QLlBHF1lJJFeuXiOgspeIkiunlCTaNkV5kFGbwXCrN2WvMnnekhWNKpbMiUMZWP8uj__eQH11S9kP3IToBDUHC9L0A)

  

Posterior al reinicio en el mismo Server Manager podemos apreciar que el servicio de Hyper-V se encuentra instalado y habilitado.

![](https://lh3.googleusercontent.com/lRXM-rjpMocIyMNddEXDMgnkueQytCkEYIj_A6N2MIxQhs-UJeR67na2G1trUlmaE8dsk2m0IjQqeTFne0joXWIQsJ8rry1CB6kkBxvu9oIebjvNZBXZIIYKjvHDXOMVcBzqE0qu)

  
  
  

Posterior a su instalación podemos abrir el administrador de Hyper-V en el cual se ha creado una máquina virtual para comprobar el funcionamiento.

![](https://lh4.googleusercontent.com/x82SzVWJOmI3U0MFT8qo_3YX7pbPORkOvuGA-mZ4TrA7OfJkiZsYz9L1XQsmDeq-HbX5jDHbpnU-0IHIZ2wh3r-6fe9n3J5iYh893J6PbTUXH14R2fyZn-FntBteh-xSZbJ8GP00)

En esta imagen podemos ver como en efecto es posible virtualizar equipos comprobando así el funcionamiento de Hyper-V.

  

![](https://lh4.googleusercontent.com/aBibaI6Uh4KtwO2VP3aA2uTeHjSVUScjNY9XZ5uQSlaSOB9arloIpVsZkjUAn9kHrqgSgET06xEl_f5YcVORoH8l8soqbiiIVd7bK2gMV_sy5_EU-rW78CH6QLiAf7Kt9JkIHgfb)

  
  
  
  

### IIS.<a name="id312"></a>

Para instalar IIS hacemos uso nuevamente del wizard para la instalación de roles y características. En la sección de roles seleccionamos la opción del rol de IIS.

  

![](https://lh6.googleusercontent.com/j1w_3k3VgYEqORm-3HlVAA4MSV2HwDYN_tQ3nC5CYEUX5bZOPXfvXvh7CyhV4DnD5djCKYJh0p4iWSqLkz-aslAqe7htX2pqShWGMieLLRRzyed_ryUgn9o_Cvut0Ea0c2MVneHe)

Nos muestra las características que son necesarias y permitimos su instalación.

![](https://lh4.googleusercontent.com/ldZm1S2rjscreBkq_VbtbMsOc-PvEAtUxRphyomVyLURkbZNZjaOCrqqlyMGjlbK3rwutu61BNMZrHg849w8zQoRfTXlpbO0LO3ddDcnrXXcyWgTbpjzHTS1yVps7DVkmuCsnOlH)

Nos muestra una breve descripción del servicio de IIS.

![](https://lh5.googleusercontent.com/XE7bMhrSjpdPLS5TJKYLgB5NTEefztC80fjd8cIbmEGDSGmp71aN82_O6OdPWrLiymKA5JXaGjiaDg9zk3XB7m54g3ZVvNUoadiMzSnvTvHfgcs3Hci0Cgc2i7bFozZiFBdGpOeD)

Después podemos seleccionar los roles para el servicio de IIS que sean necesarios para su funcionamiento.

![](https://lh3.googleusercontent.com/pw0fTC5-r71_ytf4Gts4aEo68KeB2YaBZHaev2f5Kroa8jPGYqp6wBl1eYv4I8YacQxKRIQZM3mHrpxEDjoLf98WfE4bKZ5WbnUbX1KfeCQS_a8Nh4AJOYaSeL0RQYMor239_yzH)

  

Después de seleccionar los roles nos muestra la pantalla para confirmar e instalar.

![](https://lh4.googleusercontent.com/lZqx2LjnTrI_R49Su4sPn19pmSG6GHFc9h2h_-t3LDRinEknqrsO7LgtsUlYiwFdquGVpxm6TU1gZv5UNe-LSv-_wqRzF9-AC9v1eCF1vMYJgs6yjzzblmFFd1jsl8WDzU__7ipN)

Este servicio no requiere de reinicio así que podemos comprobar su funcionamiento accediendo a la página principal de IIS mediante un navegador web y accediendo al localhost.

![](https://lh5.googleusercontent.com/J9xA0rSs6z6eyhGlOCBh8zZ5upY7xy3Y9ekKJ9QyuM92HwNsQms1u5LNMuqsn-1XiGk6zE_m0oG7nT-pun8E8CNyAafgBsCt5NmmuzdVrIvdaNob-uddOjBXB2XtlYu6yTNXioCX)

  

### Active Directory.<a name="id313"></a>

Antes de realizar la instalación de Active Directory es necesario configurar una IP estática al servidor que esté en el rango de los demás servidores, además de asignarle un nombre para su identificación.

![](https://lh5.googleusercontent.com/0PNdOqKSE8XvKv_kI1fgCQykgP2qT6nzDSnAd0ZY7MQ95xi-KLokxvPAVqAXgXZmhi_td7ce04Txc7U_niaJzhf87idUZjA3O4fqxQ9hITZaPGOPVTyzP8Cs4AyvZmej2kS3U2Qk)

Una vez configurado estos detalles volvemos a usar el Server Manager para realizar la instalación de Active Directory Domain Services.

![](https://lh3.googleusercontent.com/JCWriSa0txXGf9wSB2f0RNLvzwE7SIo3ghoPmq18rdviJHHKJmritfWGTZGlZmaHkE8_SsguA1l2paIEcVXkd1s4TpWTEWFejBbtBTgZX2Fy8XP7fkzj3gau0XVy3j7FQsDLmJz0)

  

Una vez seleccionado el rol nos muestra que las siguientes características serán instaladas conjuntamente con este servicio.

![](https://lh6.googleusercontent.com/FRVRhzZzGeDXJI8qUOE-Ak5-je2YgV2DsGiDK9N6gYr4iSt41ofVugyBQoWo7kUqF367ms2WJzdBKplCoUgul-sqQnG_NsyRPqTcvUFYey1tIyU3eWr4MmopNoZHeCXbOEytgN0b)

Posteriormente se puede observar una breve descripción del rol de Active Directory, continuamos con la instalación.

![](https://lh5.googleusercontent.com/orY9RnlZV_Y3myFdwLSZfn4GmVVy0jyNtYbbk34_q8xqyKaSwFz4rcQDDbfRfXk55UZBJTEL246cnZvFxguZPkrNgBTt8XJAXFqJv0tD1nlMTx959GqnKnOVNp_zQ9qSG6WMv1Dk)

  
  
  

En la siguiente pantalla se muestra la confirmación de lo que se instalará.

![](https://lh3.googleusercontent.com/YjtHQwBo0J2oJVbIwcOp_sBnzM5r0YhNarJRbWikul2T0SnT_2IEtRHIlFkhyEXcDgsI4la5mZk7dLmNF4o6aS79Mr4iO5yv1M4xvwbQKGZ_Q7kXsorkUCRtNtLKXN7m2V0Wb6PS)

  

Una vez terminado necesitamos de promover el servidor a controlador.

![](https://lh6.googleusercontent.com/1ZqSzga7O-XHdi4HI-rXUpAOSG_h0wIzlEpScbYKI1k6MVQ4ej84NcDT-9IT-VPtyaqJCLDkI7HivUek3r7q2gmi0SSAeiobemzU7Zol9xgtyriWeYtJTWvpuoTFlivNmmPzEN0S)

  
  
  
  

Seleccionamos que será un nuevo árbol el que se creara y especificamos el dominio que es aafa.local

![](https://lh5.googleusercontent.com/_bezXcWV5tyt0uXnUVBjsjci8d2C1IBPl42YmESQL4BGaUUaYGV-0fxia2LjJuRjEXl39gRSWD12_jKSF31mnNtsQHq8bfNti-wQbPB2-hFo8tIPvoRf6L0Op6Y2ODnGSqvQEcnD)

Configuramos el nivel de funciones que alcanzará el servidor así como una contraseña.

![](https://lh6.googleusercontent.com/KyGzSdw4YTzT1sSVg8E5BDO21rDFpqGu36QLI0f2eVNDZotrohQ_a52BV1RoaOLn1WPc6TqZSF9djsiFEhBgZrJTMocmExDDVMcNOswrUDFz88juN5oiU2t_EOkUk0S7dfZBjs_h)

  

También definimos el nombre del NetBIOS como AAFA.

![](https://lh3.googleusercontent.com/B9Uo9tsso_1GA16rigBU3rJkdb_7xHNSQHTzisF72XWKGkFBWBNWRup7DIlJhhLwTuE4m-y1RudhTCDamfXAeYqg_B3vulgQnJVNg0JkkmRhfcb-0caF1yrvfqb9tsS9kdznLczt)

Posteriormente configuramos la ubicación de las bases de datos de la que hace uso Active Directory.

![](https://lh4.googleusercontent.com/FqglrfyoRySyfFbCAneKYCUqNlvYCgX28ZcCfeiRcZZvjzGTwm4Zbu0KUZtLVkmwiPqD6iLc9YhSkHIDdwPzo-GqlKeoasGWAn6mxvoC8tOR1xAuv76Z-21rij2N6BR0t6EcYclg)

  

Se muestra un resumen de lo que anteriormente se acaba de instalar para confirmar.

![](https://lh3.googleusercontent.com/kdEKUhsoWcP-wsvX3YTcehWJido4wFmhcW5ThZNbF1hY2D9VtYw1tTHxOm6vigUf05iphpOg_GMW6dc121w771KQBXw4jvu4bdPWxqKXucM-AMFQ3NeSa4aXbtEilSQwlhxpWi0g)

Y se realiza una revisión de los prerrequisitos para promover al equipo a control.

![](https://lh4.googleusercontent.com/zPcsXEg3joNMniavnJnwHP68Vcn0ffPBwVCf55q4ZNRxkruHyhTbe6KGosDcoIvT9XP_FkcpsyP38znzTOumdIGzvWb1SRz1WoOurrDxYM4oXCDo5ruM5WFUBKZllL8UnHk6LW97)

Una vez pasada la verificación se puede instalar, en este caso es necesario reiniciar el equipo. Cuando el equipo termina de reiniciar se puede comprobar que el Active Directory ya está trabajando.

  

![](https://lh5.googleusercontent.com/d8-JyAqBRYW6cQVZAPGz2UYWXDSG0J66FL8-6sUGtKrc8I0RPoYX4eVxTs-AG4HpXKdmNrKtksZJWKCIlUPYWplMzkMLZ0p_VsipnLCSXXQjY_KvOuo2XvvSExYl5mfRXmjD6tj_)

### DNS.<a name="id314"></a>

Al igual que en el servidor de Windows anterior, en este caso se debe fijar una dirección IPv4 estática, en este caso se fijó la dirección 10.10.10.6, y formar parte del dominio de Active Directory configurado anteriormente.

Una vez que se realiza lo anterior, procedemos a agregar un nuevo rol a través de Manage > Add Roles and Features.

![](https://lh6.googleusercontent.com/Cve9X2qSVTdlyY7qBJsMmzpfUrYij_ZI4_i5-V6EDnGcXnJ4OjRfGqRrFh3abD4ClB7uOFIDilqbv7bV5mGRv0XuYVLjqj2T7G5LpvIHnoPsPEuOO5evJVvA07JMsSX0h_5TEPzR)

  
  

En la nueva ventana indicamos la opción Role-based or feature-based installation.

![](https://lh4.googleusercontent.com/_MGIiBGHIfIS9JkjFQpS-Fh2eCGflMfw-yf9vzofgA3c3_HNp0mPRpAenlJePPRAfahSJgYKBM-0C8iQE0oiNVdcJgdKIzgmFpL_iih-57kXR7oTDXF0fzqWGURUyovlW72Vmoj5)

Posteriormente seleccionamos nuestro servidor en el cual se realizará la instalación.

![](https://lh6.googleusercontent.com/pyN5B3md3qjX11rY9eKktxRnu_zZBhdOjddKDir2BnrmBG9orZCzjtoHyjhUGwfNvEcWxkK_NWP1O2KJxohFeTjX-ViSUFgjp8jJGvRiLDjCR-k5BQFnwN4OXqgDTaXzLEiKRQko)

  
  

En la siguiente pantalla seleccionamos el rol DNS Server.

![](https://lh5.googleusercontent.com/tcQcX5U-G66svz7L3GLd5iBr8I0g-naWKv5OWjUnYBYi_SJ7i1U3bEDgi5ZF-nj23qKSNlkltqL4nzq6ydcE973of-cpRuFzL_S3lP0rW1wGexTW-yI_r9jP0X5N2WhjYiojqLtp)

Se abrirá una pequeña ventana en la cual confirmaremos que deseamos agregar las características requeridas del rol.

![](https://lh3.googleusercontent.com/9Jov6lbHMCvZLuK1tbahuKjjR8cVwpKhubITPDwNaipUboipg7X_DxdFriv9fj2LcHeig42Mnea5SL7f4J0lZc2pF0oP1Wl7gR8ujPA8jBqzBflcYHkHNXinMsTezXfh9FDFH3KN)

Al cerrar la ventana el rol de DNS Server se mostrará seleccionado por lo que deberemos hacer click en Next.

![](https://lh5.googleusercontent.com/AMe5kO7kiOHpSWtK58FqF373xv-fb7TB0H3apjAfpEcH3cUdp30_uU0igIQM_znXz5qiwMgXRiNNPFQIaBPug4MUXymkcyuq6VjBywvbpV--7kjWbeD6aUvU1oZ129gLVcYe9tfI)

En la siguiente pantalla dejamos tal cual las opciones seleccionadas de forma predeterminada y simplemente hacemos click en Next.

![](https://lh4.googleusercontent.com/FUQd1WYp9sC1ILetnXuc4C-Xnc36bFNTWWa8x0jgeYMsYoHLv_2CvEMr4TeQk7y3aoD2HvT802IN9dIU7oEUlOpGeeza9bv_zncGI6ecel49KA2yvDNRZSjCFUpNOr0xxTqDFpyu)

En la siguiente pantalla se mostrará una explicación del servicio que proporciona el nuevo rol, así como una serie de notas a tomar en consideración.

![](https://lh4.googleusercontent.com/Df6fJOwtV99Zal3okLfm_98x23sO2RaAT2Dj9wQLsIjlXJA2Vf9P1o0Bmay4IXaZ-OmF8SGb_VnfsqEGZGD7AIFDtKNxuKbMoAmNwgDcMyTBQuYRQ5KbF7BRvWZRul6Kj34uJh5w)

Finalmente hacemos click en Install.

![](https://lh3.googleusercontent.com/81Wppb_DQq6AgiAQzTZc90TX87fCT9RUR8lfZQjy_RHcvYs1g_Jh18b2S35V4tQfb00IqwjGBWp7264tKr0vHGXg5baZUmsJbL3shoXY8NQJ0YUWBDGSgZHjrKWz31IfEsXStDKn)

  

Esperamos a que el proceso de instalación termine para cerrar la venta.

![](https://lh6.googleusercontent.com/1BgKn7b1XxUErmAam3LKYrotyhPYa5knxLeWWu6NkuXhbBgRxUkLXLlJbPm1K2zXhAbOEV8R7G0toTpmqxAkK4qkOfgft9ZQXxTkZ3LnO4JnPRPOUw6iHlr-TW9awU_e6Dq5Smfk)

Lo siguiente a realizar consiste en crear una nueva zona DNS, así como registros A y PTR asociados a equipos que deseemos administrar. Para ello nos dirigimos a la opción Tools > DNS ubicada en la ventana de Server Manager.

![](https://lh5.googleusercontent.com/Kg31XsBnCa4v0Wf5K4xrEDs75Z8wfgnoo5fWI13dOL7Jh5ORTobqTYVzP2S4byAYUuOqW0VSvSzmqvLUXmd7DlcBm6fiTA-_TYuj1Kk4nPlQPKXTU_69YqUyHaewXUHD8EVfKPQN)

  
  
  
  

En la nueva ventana, dentro del panel izquierdo hacemos click secundario sobre la instancia de nuestro servidor y seleccionamos la opción New Zone…

![](https://lh6.googleusercontent.com/4WXWo8buaZC1i6g6NgDN_iKlnkx0-Gw1ML9BNgB2rNjVkRAgCJ3wCv2f7gC0_eJ1XtUPgoRx20zJ5RtLOrJJVvwQL2AQ0e77YZR3p8bJS6eGQk4LKkFqvGZnANJ5jlNA7o6tOkQZ)

Se abrirá una nueva ventana en la cual simplemente hacemos click en Next.

![](https://lh4.googleusercontent.com/oTDxTAGnCx2rrFibNki0zzJFtxWmY5Xn30ssLoOHW7dQjgTl5ynJg2nAbeAtJrsHxWLFk2RzidJBCtlfrsKWL0OE7Yqj6R-5bNzszcKla66x3VOj1nb7d5cUGCWE9lx9UnSM2fOO)

  

Ahora seleccionamos la opción Primary zone para la creación de una nueva zona primaria.

![](https://lh3.googleusercontent.com/_HSEeDhnxohuIqsautI8dPA9y1jGkVFzwjqBAIbjNR0is-pRygqFmSPIF7swTCuYOyVbZ-rOJm55vjoHGWDrc8gOG1Takfe3OY_mRrnlc-G1V6bk5RMEMfq8QIDty62omlGRcAqJ)

En la siguiente ventana seleccionamos el tipo de zona Forward lookup zone para la traducción de nombres de dominio a direcciones IPv4.

![](https://lh3.googleusercontent.com/3uQzy7xkCuoERY8i4ejjOjE4nmWWGpavQ4UcTbdDU9fAVZ0xwYeyQ6AJjvJYS1iuYOROrdOpPInPckh9jEnvjdqu_h9aGyn9AwdXp22SeW82ypWu9VTHhMJzyPB2Z5zXmrG9l1w7)

Lo siguiente es indicar el nombre de la zona a configurar, en este caso indicamos aafa.local el cual corresponde a nuestro nombre de dominio.

![](https://lh4.googleusercontent.com/DHUUzXb3L1E0vPKXH0L_uIa8ByLQaxD5bmT3bRVMyGYfiOgCLdNJ9i4GrcoMe-KctH50vqXK1vC7NJ7NXcjvzT-ee5zqXncRKuLzYvqF686H-KchSrUi5YgPRtNzHyRh3YKBPp6A)

Posteriormente indicamos un nombre de archivo a generar, en nuestro caso dejamos el que se genera de forma automática.

![](https://lh4.googleusercontent.com/uPp0uD24cWHlVsxVGRUKW8C9rscjMAbkxX4-MfeGiHNXIeY4byIIIieBq9Yf5jQ1e-xJ2qkbbI5OAYZ01Sf830HefeTAa6AoLNQW9fA7I0FT9fB2EcKeFQvRXMDpOITvI67Fa_qN)

Luego indicamos que no permitiremos las actualizaciones dinámicas.

![](https://lh5.googleusercontent.com/Wnu2HBpNoHbQqlkhIUcC3_TR2q3aPIkfJlMRquKu4_R38ZUmiaPJV7GxnnTAJ_i68u6p4wnk8fAOOg8qB6O3dLSp7QnQ_871NQzIFHN8SMdeic2gJCUN7aMhsoXXulgW9d5q6Y8e)

  

Terminamos con la creación de la zona.

  

![](https://lh3.googleusercontent.com/4nZ7ZipHUlYItprvDVFU9IVBocHboHhHoTGFLtwiJjCUD7c3uz1yHDYt3Ad_pA5Ry7a6KASOuAfkc6ddm-c8SgBR9Nsd4qjZoBV9KhnS9GfgjAKV3TdbFYGjjvLxJxV_ABflBd4A)

Ahora deberemos repetir el proceso de creación de zona pero para esta nueva zona seleccionamos el tipo Reverse lookup zone.

![](https://lh5.googleusercontent.com/x80wxvMFDUj3iSUNZA4ivlBJ5rgYX3ZAFBX0HiizB1Cj5zY20Q2zskIpcBLu-7XogaBBKDh7Avt7IEH5GzkGOoFl7YI8h25M5biTSpx84jldP4DL41EAxVS1nIV_rYlOIRWyGC8G)

Indicamos que se trata de una zona para IPv4.

![](https://lh4.googleusercontent.com/FPMTeo0xHpbaXzbmXdvIiPEk175T2PjNMHA7GyxjuJWLnog9D2zxcPphsbt98mmONY0Lx7eldqlzfZvkTfwuuWhyY6N2tcHaZkQDHt4_rdZQ5MXKL-f3jyGpCCA0-GZNvc3ElGOP)

Ahora indicamos el identificador de nuestra red.

![](https://lh6.googleusercontent.com/me5r7IyUp0niDCcIEejnu2jS-E9nW90Majno4KnM_QkwCfAgII2XLcSzkV2ArOr9w5qlLsNp-jWoir0ffvU3xePl_LpopD2pp1mcgA8ZdgFCWoskA9y-oXf8bRv010Ca9JBaa3mu)

Y establecemos el nombre de archivo a utilizar, en este caso dejamos el valor por defecto.

![](https://lh4.googleusercontent.com/duyamqWA1k2fEOESZynKMpYcESOHL5cjZFX6a8xf2QsXWhEs60rhjUmN_eQVYtdUDjy_2I0AnZp-FwJGSCIFDaRA34iYryFWXdhKWvFIsMPZbnsJ18O9xOjOOGMgS57v56W16n-1)

  

Igualmente indicamos que no permitiremos actualizaciones dinámicas.

![](https://lh4.googleusercontent.com/ScQBayJlXn-I8hBDyPayXzD787-SB2F1SS3zdZifIbesQLsGaNqm5wpUqAy2FTjWY2fsNO_Wt4JccdqWgzHKkx8NnKAwgvuO1_RBYehHO9-1gaSjgixvycFpvduOnhUhmUGj674-)

Finalmente terminamos con la creación de zonas.

![](https://lh5.googleusercontent.com/YtJCFKhHm5KBEsGD4bLc8Vln4CKkzUUZ92_-GO-WmghsfGMauykS9oJ40Wiejyq5zm-XcaQGmy8mpyvfuzwSq8A5AXF6MqtUiYkfnIPVSmG7eLi_3BLEabN1DV3GQyB2Y6IIMg3t)

Lo siguiente será crear registros A y PTR asociados a cada equipo que deseemos administrar. Para ello seleccionamos nuestra nueva zona aafa.local y hacemos clic secundario sobre ella. Dentro de las opciones desplegadas seleccionamos la opción New Host (A or AAAA)...

![](https://lh6.googleusercontent.com/20QZnoSs9LqkmgadraYjqo_L7w0eeeSNuQaZWLxox-bsSCvh5RbMsQ4jNWxnBAxr-sTx8m-uxJhMmJ-BqxFnrykt08h5Exrcyzq2jwBBU5XzxprW_HKWqmwwgARkL0zCJInLghXH)

En la nueva venta deberemos indicar el nombre y dirección IPv4 del equipo que deseamos identificar. Adicionalmente marcamos la opción Create associated point (PTR) record para que se genere un nuevo registro PTR dentro de nuestra Reverse lookup zone.

![](https://lh4.googleusercontent.com/KZQfvGCnM7UyssjoJ1RyRqFkZVofMn6r3QdWcnNxRyuMEWpZQcE1qS8MlezBed3dru7eJjTnmb4yBtOZ_1Iad9mpun1BVVd8qQxyn8xWN14gFTAIDZlqyww0f-y_Gha0ZXu-RpwB)

Hacemos click Add Host. Se mostrará un mensaje indicando que el registro asociado al equipo ha sido creado de forma exitosa.

![](https://lh6.googleusercontent.com/IFnVvj28KRHEB_4R6_KhHohAvHaPMtPc_CjJFL5t2iM-MOMcObhrpPN0DOiEa8BFnCF0sdT5fBIH9aFVJozHjpId8tPrmAGhZ9Yl-09q8BlmudUEScDtK8at2UbLG6vKQJDS0yBA)

Ahora verificamos que nuestro registro se encuentra en nuestra Forward lookup zone.

![](https://lh6.googleusercontent.com/mt7FpV4cizWMUhzAufcErnMntu6ffYuNXNpoJkrXNIkkIbDMFQNFlD2fHzoXukzOkBCSFdP-t4U7e8w0cqNd2mgFeUfdPYDDa45v6NMGQlrRAIQeVv9Q0OqWYPQz9X4omn24m-kW)

Y finalmente verificamos que también se encuentra el registro PTR en nuestra Reverse lookup zone.

![](https://lh4.googleusercontent.com/A57ocl6hm6iziiKA1y-geDkgwvBJ4XOzE3V3gcdHfo5krK794H9n0usNkSRlzyvLAVxyaLYusvWUpiHxveb9-IKtB_FsbL0Dhkvo37lpZ07brzDmelMVFiSSP1Jer1lgaXluPSDG)

#### Habilitación de logs

Para habilitar los logs del DNS debemos ir al Server Manager > Tools y seleccionar el DNS

![](https://lh5.googleusercontent.com/haHnvkMc9msvedsqrwp9Hna_FHRq2JmhIxd_WpZUYQZRSI1q8J3m5Jv-bp8P41VHmCSBK13_fohf_rfWCY1szb895miDcTV9Jp75UOS533rbNHZhi8QmnTyOSDHjCOKvtmK-5Nx_)

Una vez abierto seleccionamos el DNS al que queremos habilitar los logs y vamos a propiedades

![](https://lh6.googleusercontent.com/yznUx4ruSBr5pstoYCOYOwg6NxynG_viWeC6DB2vLb9STyHJjPtZCj7mdBgUwUgl2Ygkw0tLOaGoxsv3IkfmNf4ghLs6ouqcRUdnmsmMBygBqAZwy6slVGGqyD7i9EPkWhJUcSR2)

Ahora vamos a la pestaña de Debug Logging y seleccionamos los eventos que deseamos mandar a un log junto con una ruta del archivo

![](https://lh5.googleusercontent.com/rEYAFMsq1HVObrubTIcChBk9fuzBr6KQiZkw86rMhYXRcUZzNCLrhylV4C4VwRlveetra89NNwCHcSgbDhqyu4HTkSMkGvWAi0_Qfl-2Rv_ZfQp7wfe0gTHY8jt-DCmk4nehc3Og)

Después de reiniciar el servicio tendremos el archivo de logs.

![](https://lh4.googleusercontent.com/tCIfNWUusH93L2IrTMbBw89jHj0YnnHxtPPc0yn3wjSKbbLtW7Rymr_o_ntIwMU1pwLFsK8Eq53zlwxncLz-_meFInQOLPfcD8iSlArRX0D3koosJpjcSb5q2y4SxU3ngjlgCU-Z)

### DHCP.<a name="id315"></a>

La instalación del servidor DHCP se realizó sobre el mismo servidor DNS de modo que no se requirió alguna configuración previa.

Procedemos a agregar un nuevo rol en el servidor

![](https://lh6.googleusercontent.com/Cve9X2qSVTdlyY7qBJsMmzpfUrYij_ZI4_i5-V6EDnGcXnJ4OjRfGqRrFh3abD4ClB7uOFIDilqbv7bV5mGRv0XuYVLjqj2T7G5LpvIHnoPsPEuOO5evJVvA07JMsSX0h_5TEPzR)

Al igual que en el servicio de DNS, sobre la nueva ventana avanzamos la pantalla inicial, indicamos una instalación basada en rol y seleccionamos nuestro servidor actual dentro de la pool.

Posteriormente seleccionamos el rol de DHCP dentro de los roles disponibles.

![](https://lh5.googleusercontent.com/fVMOPHxZEs9ai0betqWZkHd2Xh5ryz8xaEqUbQWnQ9ciMIB6qKQhAu-mZ4-4r1lwth-cx0D0EagkYlWA2EDO5EWCFdovp19Qa3dvK52y4mNsgu6oHSBynjEUZ1SieV_qDmRUPp3K)

  
  
  

En la nueva ventana confirmamos que deseamos instalar las características requeridas.

![](https://lh5.googleusercontent.com/rzYZEW0jjyP1gTdw5rrexziuoBZOdx5HQOC-bEiZypNFRrJqlv-SU51OB1M6_PjKYOncMzVx__xjrQgiVS2kbI5PHNW8Of3Mfhkv861oHTVd1pVMPtz7mBF2XzrkUhfYDMFWHPiP)

Al cerrarse la ventana se mostrará el rol de DHCP seleccionado.

![](https://lh4.googleusercontent.com/ZwWCwT-iezuFW4PgvSHz4qyQ83f4qlKIqKPeCyL1GdaMNmXUXXcTFWkTnuOyzI8HvTyEy7_aVbzYn50SxFvC1h2x7eDjHOAyB2YyoSOTim_sOmbBQYAiz9_ItuEUrtJvXq_iX9oe)

En la siguiente pantalla dejamos las opciones por defecto ya que no deseamos agregar características adicionales.

![](https://lh6.googleusercontent.com/4R9rdHHsVSeHyGyojMeU4pIlCk88v1AQp9Ljvh_Sy4jEoOkrJqtFICvNBA9A71V6aT3lAfUgWF2S_mikEvrkDp4ubDmaqREM-jBjMUzfWSjGzbV0Qu00BYTFYW4NTy49uAlUc_SS)

Leemos la descripción del servicio, así como las notas a tomar en consideración.

![](https://lh3.googleusercontent.com/yvMQ00ovWDLaPFoC0LnJcvMUdJyonHXjaCQ8r61ATPHj4RmZHlSls8YuoZU539sKb3ijPWXbwZCxp3bcNyeFMfqjIZlwz7JeCPQcHienp9JB0QOkZ3LX-2IcudGqnasycQtrndKS)

  

Confirmamos la instalación del servicio.

![](https://lh5.googleusercontent.com/X2_pTuQh5Ts_HuQdgC5GK0H1efCPmut7t_NfVLdOjeIV4ktox0QxcNToBJJbPLtvoSeRTdiErqRCOsl_YnVNIV3F6ifM3kKDXq3Z8QcFJDPiyYx2kX17WtAqMpIA78smPLdGHHU_)

Una vez que la instalación haya terminado podemos cerrar la ventana de instalación.

![](https://lh3.googleusercontent.com/6cjkYDTF6PmuQKtb-XmzhMORYCy6KiFoUzvBsrI7XpDFHbS6N40Pkpy6xVZ95iP282MUDO28DyU54ccLa6lXDLX9imsSaDIEOM9wqM_UZP-MrHRRYa6bJAN5qnPp1vvXHWID1xoO)

Una vez realizada la instalación procedemos a realizar la configuración post-despliegue. Para ello hacemos click sobre el ícono de la bandera amarilla dentro de nuestro Server Manager y seleccionamos la opción Complete DHCP configuration.

![](https://lh5.googleusercontent.com/2KXjOc8KimPm0pSjM_uzMjiqxF_j_hoEF6yxHgDVMFSu3VIjnQFkyKbAdVq7bXXMEPsHd49j9jAWFZs4SV74aNC16z0nL_T1Yls5kNu5cq_jM3FE1jicBd3W-fMz5rR7UJx9nMMb)

En la nueva ventana se mostrará una descripción del proceso de configuración, simplemente hacemos click en Next.

![](https://lh3.googleusercontent.com/3dr2SdEHJcYmU8VkXf60NGZvrF4h9NlzUQfCHqyUNWT_-gOIurRjQdn-hKZFmDkLfQqBTWPBBEH4asxo9Of8t1ipkQemRq4RKKBTojCpxYBsxBwpoyFizLBKJJo2Gh5t8bsUu9wE)

En la siguiente pantalla se pedirán las credenciales para la autorización del servidor DHCP dentro de Active Directory. En nuestro caso indicamos al usuario AAFA\Administrator y hacemos click en Commit.

![](https://lh3.googleusercontent.com/h6nwlIsp5ol3RuJC-72l71BGCdMQwYeZShR70FYk3O1YZuFjeLcrP0zg8xjiANFFfNc5UhlFklvmZghV7dpfi8j9SJ_fm--0jBsClG1DEIa59tJMElsxIHLwLaqQ8y1rGwy-oLzI)

Esperamos a que la autorización se lleve a cabo y cerramos la ventana.

![](https://lh6.googleusercontent.com/R06V5H7iw2_tMOsvcoq6ofyDYrPXNC-p8EvYy7CSG-AoGNSXiXG1sw7dTXoxZ0n8r4SxJWy612WeW9iS7Dm_DBOmbx1jm8SXM4sF9wIeAYbcb2iqoS_nFurrT_aOdNF8eaonFsQk)

Ahora procedemos a crear una nueva pool de direcciones IPv4 a asignar por nuestro servicio de DHCP. Para ello nos dirigimos a nuestro Server Manager y hacemos click en la opción Tools > DHCP.

![](https://lh3.googleusercontent.com/_jS2_CMKOomwXcPRwtNZaOwBYokbJkGiAA3RqlrNrpxeOxic-zoctOj3U4tDGy3Kv5aVnKhG5BHPCuHXR1NSxt4gpe9ulOY393kFhucMFkTOJsJ5zXkRWJwWfVbLTP5hEW2Na3N1)

En la nueva ventana desplegamos las opciones del panel izquierdo y hacemos click secundario sobre el ícono de IPv4. Dentro del menú desplegable seleccionamos la opción New Scope...

![](https://lh6.googleusercontent.com/-FtXugcz9bPW8nR2OMHkHwV0YV-QCB93REitQ3hSa312omUQnDtEqpGn9pwWOEJtZpWGT-kJWkz97vQS2E7XjyNZsJ9n_ohQu3PCSG2ExyzHzUjnu4d-EFqmT8b4f01yeYL262cz)

En la nueva ventana hacemos click en Next.

![](https://lh5.googleusercontent.com/oBcMW2Y3vgKi9zrDt38nkHHULpcapU5RipsneJwe4b_eeXBnJDkHUW6-5Tl9t6j8wUp3TL9UdRyMbmuDmgK5OlwozoynnBaY-VDtA3HYJTFk-YvXKl1ectALJi-w1o0pM2S_aW7J)

Ahora debemos ingresar el nombre y descripción de nuestro nuevo scope.

![](https://lh3.googleusercontent.com/fGJ2Sulqx86Cu_NeUNzcjeB2SnbRXl-bUhXeUAwo8gZaxtmlju1JtMiNUvdKrPyLi_NuinPsJ2r3JXJXyPQety2YlBAbhNvNIK5OMkMIhqzso7QI3rwTwB53dJ4Mu7aR3CuwkDhz)

Luego debemos ingresar el rango de direcciones IPv4 a asignar, así como la máscara de red a utilizar.

![](https://lh4.googleusercontent.com/pKu2aJ5RQyCp9o2FA5hsZdKc9twKnfCWKTRGMPaZf0rhIbJM3gCvhG5SZQaRo2gaWtk9qcUByFVDj5TBwU-I42yY8FR64C-TvqcnFQknjZOnMWnWS8X8WecejIJ9gcQ2_reu52si)

Posteriormente podemos agregar un conjunto de direcciones IPv4 que deseamos excluir de forma que estas no sean asignadas. En nuestro caso excluimos las direcciones IPv4 del rango 10.10.10.5-10.10.10.31, las cuales están asociadas a los servicios de Windows y Linux establecidos en el proyecto.

![](https://lh3.googleusercontent.com/v6Kfq9VSDp1GwDqNKa39xYeKHyzBBJaJSyRAcxCNWO4MIqwm3fAJjiJEH4XP1uYZZnbEVqdYIrYXq_j5BKDfvZH6Qyev0OnqidCVj2o9naK-u-Vth68MBIxvJ10CjIng-HFwBs5b)

En la siguiente pantalla establecemos el tiempo en el que un equipo puede hacer uso de una dirección IP.

![](https://lh4.googleusercontent.com/hEfQTD_Vg3IqjwXr0_u6AuoiXyM5Lx3NXIEUlpYFSwSyStSR79fC-7veoUrn3KpSKoDTEL2SIm3WSUJq4ny7xnZwRZjzprAa3NO-S7EKgLzLSIZStFHJUIfOWq4jqsYHiv2y2Nc-)

En la siguiente pantalla se pregunta si se desea realizar la configuración de la dirección del Gateway, DNS y WINS provista a los clientes por el servicio de DHCP; indicamos que sí deseamos configurarlo en este momento.

![](https://lh6.googleusercontent.com/ldn62ZOCgrmiw0rcbsPZdB6EHGcrGXb0_37TwP5PjoyjS7OLirwdHQMR2dcdcDbrzI4j6RbEKS0mo-GJdpdUPmiQtNAxDWvmG1POYHTStGatSxjgtdbQqdyhvVA3AsZN0dnj-i9L)

Se nos pedirá ingresar las direcciones del Gateway. En nuestro caso sólo es la dirección 10.10.10.1.

![](https://lh5.googleusercontent.com/7refEmcOPrLmCBFMo_taT3n1H86I1x2kfFq6fmVxErBcdPNfrSa9GlkX6Zdo6Z-4yFMUNeYWiuPcJrZ7AymajShCIFbQTWNhh2oeaf4IUr17aJ65lMd6pbv_hjXIpY5G4SNunRc9)

Posteriormente se podrán ingresar las direcciones y nombres de los servidores DNS disponibles para el rango de direcciones de nuestro scope.

![](https://lh3.googleusercontent.com/pVhTN2fjvYjNSbxHtTynrpGXCunq3f1dTaRXGcLknfd5vj4Gp497cxrz5XiEXaLrd6TyXk70Oj2MXfIR_fg2w3rllIdFJY9PXTauPhTkx8s6rikIvQnehtFY5WLOGtSfTYeeqtnr)

Adicionalmente podemos establecer direcciones IP y nombres de servidores WINS para la traducción de NetBIOS a direcciones IP. En nuestro caso lo dejamos en blanco.

![](https://lh3.googleusercontent.com/BOt_DB7AC8wGrataPAxtlnRkXoaenI5gu-62Bcnv1SIemT4DD6Zp-S6bsXSKbe0bCFopoWIp6SeLMe1LctkAK5XwfxcPlXs5eRI04iwk7tHyKmslx46OxJLnPfmWagx-Uu34m54o)

Luego indicamos que deseamos activar el nuevo scope en este momento.

![](https://lh6.googleusercontent.com/APjJDh7EG8tiylCis43MinWKWjYBedsSBHZ5lDdnO238vAOiN_shacolMVwksYFQIkSpMfEBSEiIBXMQTw4zJmk7fJDR82rdEDHbr9881UZyxfS_PXNrxZ8epc5cAcXYuM6yufMg)

Finalmente cerramos la ventana de configuración.

![](https://lh5.googleusercontent.com/oDJGSOFjD27puFqUGTR1baIZ5YVfNPxzw4TFyeR8vHCkUWZ8no9bOWOxh5tuWGNSB3wkUsnQxsBPCl9JM6ax9hRqejT6om98I_iYvolb_uuu4GbDPH9yb0sondeI6ctgHV6jIRru)

Como se puede observar, nuestro scope ya se encuentra listado en la sección de IPv4.

![](https://lh4.googleusercontent.com/ysLi-nKsG_Px36YSbx0Uo9mGaK4YdrsOvgE5HFVFfg6icEqp6GISH5FuqitVHn4aXFXjGjZSEqlBXHrV4Py15ymodByjqNc4YwjftUQaW-IjdGma1Vmup-i4xjyqXhfScuJHLxum)

Habilitar logs

Especificamos la ruta para la generación de logs. Además como powershell advierte, se debe reiniciar el servidor para que los cambios tomen efecto

  

![](https://lh6.googleusercontent.com/bnveDaYpm6XTrkLPUFHyAulo1TsATxU6_9wkwrbtCBaM4WjOmHkoFbOsDHNMM78tL50YV6aEQazloVDNUwe73qaktaxzlAl9Qh4XyyBqNCX1fvBp62Z4PsJwtUaIYfdeKKTuqCHw)

  
  

### SQL Server.<a name="id316"></a>

La instalación de SQL Server 2019 se llevó a cabo en el mismo servidor utilizado para el servicio de DHCP y DNS. En este caso se instaló la versión Express, la cual puede ser descargada en el siguiente enlace: [SQL Server Downloads | Microsoft](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)

![](https://lh5.googleusercontent.com/VHfyU257MpmwgqM6tiJwxXBon778T-CYzsutbkR-CCb7spjQVfHkb2yt5TzL9HQZibvtPDQEjA-6nwXcggYpF6qBvSX0OfAkix_oTV8sNuECmHPHIzFppa4eq__W3ij5EbqkUsXa)

Una vez descargado el archivo de instalación, procedemos a ejecutarlo con permisos de administrador. Se abrirá una nueva ventana en la cual deberemos seleccionar el tipo de instalación a realizar; en nuestro caso seleccionamos la opción Basic ya que no requerimos algo en específico.

![](https://lh6.googleusercontent.com/nbqxVZFSP77e7ATSfK-OCjJF3iZwMqDIqCyTv-bjDPKHF7TqmOTGHIq9zGx3wgWSHnflSoR1WsDE61YR6PkceaesSzQstFAmdYXXv8XZDXcJV8VBCE8nYXYO34XbNM8Ow_KaMR6V)

En caso de que haya algún problema de idioma se mostrará el siguiente cuadro de diálogo. Se deberá indicar que deseamos continuar la instalación en el lenguaje de Inglés.

![](https://lh5.googleusercontent.com/CzWALE-7LJ2UijFbmF6EKfCrWNPuypDKMDGmKR5d1UtGtBHqkN41pt-vyj7WFifd1n_j5udeG2lQL-DRklToK_BdCfrAD8mm32x6Cfa9mD8AnCggOdb-kWx1-ZItGn2xRRtscIhY)

Posteriormente aceptamos los términos de licencia.

![](https://lh3.googleusercontent.com/nto9cxRyALEJzbsx1A5eLMyak-YhXKGDs5rzf3X8BjwtArfOcvMS-xFqfoVq8xvcotuc7wrBzj0ZWnQ87TmhCP5J1x2QNood0VpqqmA8Q8GYpT5LnV6c4NGXypoi4UuNIWvX3P_l)

En la siguiente pantalla indicamos la ruta de instalación la cual podemos dejar por defecto. Y procedemos a iniciar la instalación del servicio.

![](https://lh6.googleusercontent.com/PXyfEmBJygouYGp35nB1z25FK5_zpQGFup7k3fn7pKxIXADdtWNx_7pCl-tO_irrACt5gdeKAUX9ejlSihme16lt96J4TTPBvLCoDa_GmQvu0f_JtEr2bpALj7FztW4WBtAaNZrn)

Una vez que termine la instalación se mostrará la configuración de instalación final. Ahora procedemos a descargar e instalar SQL Server Management Studio (SSMS) 18.7.

![](https://lh3.googleusercontent.com/mTH8dkt2oCb9ZnyvuU2KeTPIyLMFpzI5POJXr6M57xw33Oi8thutuywjtDxjYgeNYs2p50-k4RVe1kPZP0SbLqWwLSiShNOhXuK9fZYAK3myXQT8LupTBSo2xvUEdSuXdZ-a1fa7)

Se abrirá una ventana del navegador en la cual deberemos hacer click en el enlace de descarga del SSMS.

![](https://lh6.googleusercontent.com/tYQtCzAlTrIh4eZc6atPzKISv3rvYC2baZFRqgX6ksgCMaSgMkkcm2kjbLYm-vckcoBk_aOsVAOjhyiMPYqy0GI2va6uPcgXnxJyaMrl3jgr4NebLgNakWGKpAE-GSqbzLwPMBBL)

Una vez descargado procederemos a ejecutar el instalador con permisos de administrador. Esto abrirá una nueva ventana en la cual se deberá indicar la ruta de instalación. Posteriormente se inicia la instalación haciendo click en Install.

![](https://lh5.googleusercontent.com/4BfBxi6-Pb2z9MrUd8JMp9h7WdMtBYSF58d_aQM3tt_JUBBRYH8X_PgP1JpV3qpFRt4FbVtOxwMApFcnNCXH0wVd3R3ktQcQcvHWTC1hLzyLt8Qc8EqF-pDRbQH0Xa6i4oq4AX7v)

Esperamos a que la instalación termine y una vez finalizada, cerramos la ventana de instalación.

![](https://lh6.googleusercontent.com/Z3AhSTbUQFZRf694AbNObZCVPMVia-56egWV3hxK97PZmp1WZqwFEV1cw7z5jzNIw0e3MzoWcGsVXSv9mlD9odIxxl0DufE2C0INjwdKQeHCxoAio9aRmItPyzQInvooOvlfUT86)

Podemos probar la conexión a la instancia de SQL Server a través del comando sqlcmd -S WIN2\SQLEXPRESS -E.

![](https://lh4.googleusercontent.com/DLAqPY5goaU4CkHzwBxakmeWWJ3C4d8MBO4-Y-rSE0Zk7kkpd9-Hbk1--91yqsahMvuRG5F3fQe3zKFY6sYzr9HM3a0723--IPHp4Nqn5faUWIMBmsv_DSUHKvh8oEvt4ZM85Ivv)

Adicionalmente verificamos la conexión a través de SSMS, en el cual deberemos ingresar las credenciales de acceso.

![](https://lh3.googleusercontent.com/fhswak8c55QXrrQD9j_Y85B-RF2BGIgApLIy8LGG0v_Dqpe8wbIfUzOCLtU5Vpdx7iumpJlOqV1ZN4KQ8Xq3EH-6i4yvaKBbL7mltjbyLw-6OxeH8SvoZzM7IYceuYh5tCizTNkM)

En el panel izquierdo podemos observar toda la estructura de la instancia de SQL Server que acabamos de crear, así como las bases de datos asociadas.

![](https://lh5.googleusercontent.com/hg688NRqKAsP3FagNfJCmo2FoV7SEdVMQmliVbH4wqeLxVs8ZT7KRe5nbEVSsfDbRMLOyCS6AZYikWpne8-KTFZwQzqVCIL_UOXSkhZnNbJD9SzAhTLT0I8jT4bl3U3KvFCZDDAA)

## Linux.<a name="id32"></a>

### Correo electrónico.<a name="id3201"></a>

Se instaló y configuró un servidor de correo electrónico sobre el sistema operativo Debian 10.7.0. Este servidor cuenta con los servicios de correo a través de SMTP, SMTPS, IMAP, IMAPS, POP3 y POP3S. Además, se configuraron e instalaron los servicios de Amavis y Spamassassin para el procesamiento y manejo de spam.

  

#### SMTP, SMTPS<a name="id32011"></a>

La instalación del servicio comienza con la instalación del paquete postfix.

![](https://lh4.googleusercontent.com/gT1kWrXg_dVkfeWUPF9rTe5yw5Rqv8QRVhvzfDPcrwy-GLV4uGydL1i7UFBUD82wq2agYYhS5TGl15eKKdbpP-YAnk3DzAKS6EtIx4dO4YBn15Qm3Hw7mi36fG2w8zfJzNnnQaAC)

Se comenzará a realizar la descarga e instalación del servicio. Una vez que comienza el proceso de configuración se mostrará una ventana de configuración del servicio en la cual se solicita el tipo de servicio a utilizar. En nuestro caso seleccionamos la opción Internet Site, la cual permite recibir y enviar correo electrónico a través de SMTP.

![](https://lh3.googleusercontent.com/E8Y9AkJCvwqjb1HFjvDesA4kgg7CoKKFLoSHk5_ay9FatWDaTjGmZiGfXuNi0Zuz8CcF-fqpZQbLP6l2IkcJyhnvAiAl0O0-LHhmu_Ep-NG7eUwcs4mNTKyYklEOUdPGNHqXQgya)

En el siguiente cuadro de configuración se deberá ingresar el nombre del servicio de mail a utilizar. Nosotros ingresamos mail.aafa.local.

![](https://lh3.googleusercontent.com/iCtpcD8GdPCkFo1cgLedln3KijOhhqPlGhysn4JabidQZ0iHQaIwdd4rb1JvywwwQ3rx7A5CJA75YKbahV6Zk-K5AG3zPAtKjKtiZHqalOGHbwDpau_HSNMMOd7_zCCyFZy-N2-x)

Una vez realizada la configuración inicial, se deberá realizar una configuración general del servicio de correo.

Antes de realizar la configuración general, primero generamos un respaldo del archivo de configuración /etc/postfix/main.cf. Luego de realizar lo anterior, ahora sí comenzamos con la edición del archivo.

![](https://lh6.googleusercontent.com/M2IYesdYCl89JloMILb1q80zLPFjLezP1cIdwnSVGM295avfa0xhoiX-Dg9NaHGbNGwJGe1mk8nSw9wjQZZmCEmzvy0MGf3FwV8UT47E9YCpzGR0OqjQlJGwkXOP8XNeT8i4dUHO)

El archivo de configuración ya contiene información generada durante el proceso de instalación, en este caso debemos modificar sólo lo siguiente:

-   mydomain: El nombre de dominio.
    
-   myorigin: El nombre de dominio a utilizar en los mails enviados por cuentas locales.
    
-   home_mailbox: Directorio en el cual se guardarán los correos. En este caso se guardan en “~/Maildir”.
    
-   mynetworks: Redes locales o permitidas.
    
-   inet_protocols: Protocolos de internet soportados.
    

![](https://lh6.googleusercontent.com/BjkjvABgXOajjZ7KRi3IZzJjDRTRe8mLU25Eb1CTet-DEvcmPml2wexKzGj2dRieWhxJCUjjOAD1gB2TXunbrO4x1B3tjz0-GoEu-AjDkAqPcjPLVlIJo2cHjfeXr6YAGFtmf3e1)

Posteriormente debemos indicar la ruta en la cual se encuentran el certificado y llave SSL.

![](https://lh3.googleusercontent.com/ejRdw6v4dHQobQyUiwqu6or0M--MaOCZhir0UsYvHUK5WvWOTuTFCyiZ1JM5Cl80MEtizG9k5tIT6rdrTBn7xqc7q93j8dDqV5iVRS6sen-bobJm4U39_5kJI5AeLcmMxfI3xxyR)

Ahora se agregan las siguientes líneas para indicar el uso opcional de SSL, las versiones soportadas (> TLSv1) y el nivel de bitácora a emplear.

![](https://lh6.googleusercontent.com/2Vz25i9xaiT8oWrtNsXXtbzBLpYgBhvvQCXbHlC6a1PggKl2E7vq7OXTm6pghSLlFeWj_zIMlkS45zMsLwwSeRrUfYk1BBAp_WeqtKE1bZxPgmhPmgB_bNfzHZ-VRXWKtqjnhmAE)

Posteriormente se habilita SMTPS en el archivo de configuración /etc/postfix/master.cf.

![](https://lh4.googleusercontent.com/NXhlZachQUBbyX0Wt0Csj75FOymszHxCqnwKORlcG-9zhog8empXCyYaDFjtbI9hFOKqt58vVbMTC9QE0KpU9Wma5A5qRKjwC8iN6lYHHr2W0ygFaM1lv7JvlXz9VgZOFVAubbEJ)

![](https://lh6.googleusercontent.com/OjsnKNDdN7T2gwwn5XkyYRpougd5H10JjfM7xRpUlHkn2M5SuHPYryx_XWVvujNz_1Ym7VOG_UJLZYx4PI6qxycAcdcjBgdVV45b2hn07HyL6yMX5oSM_ckQSKkOWmA1JuiOBBJQ)

Reiniciamos el servicio de Postfix.

![](https://lh5.googleusercontent.com/uOQcjl6jFjXT8lBfQclGL9KaKq-GqikagWv330c_CKdOLhfxWCuQsx65-NuGgMXMOI6ttFXpOFKx9qB8t3Gz4iu0L4I0J59o-7GC2ZQlzDOmv8pYvJyCemLeWuI7bps2K0WQ_c9B)

Ahora probamos el funcionamiento de SMTP a través del servicio de telnet. Lo que se hace es generar un correo de prueba con origen y destino aafa@mail.aafa.local.

![](https://lh3.googleusercontent.com/YLnIT44p17vCuXd9kA7C7TLcvFQWpI-w3yotYKXyGqnSZbCreUpnBUuIU3Aa-U6xRKcJPtR6ys0fAjmrkL5h2-Gp99pHKBHzYI1wLNWfSERuarbGRAQgnhhiDxdz82TsgYzYHHZl)

Verificamos que el mensaje se encuentra en el directorio de correo del usuario.

![](https://lh5.googleusercontent.com/drZOYsqf9mOIVx9t5cJuulY0pquASRncImLCYLWJ0lBj9btsmxDmPBcbh7m67oUjIJ9IkYsG2yg1_cX28gnO1yA8c77MFfF6WYbPsHynH7VKUgin9OkbQK9X6DM5jxkAAf1SJIko)

Ahora probamos SMTPS utilizando el cliente de Openssl.

![](https://lh6.googleusercontent.com/00E5myPKfpc8_pLUE9tvy4_FqxHOa-jsZmrZ3xKgt_nZEbVGMYbU0iiX-8uwBXZsWhieYQhY8_w42C3IULvrT3H34NDBWmIwm2UVIxAV_BedYq4d6Uo6seAOYHpksIu_8Xi04os5)

![](https://lh5.googleusercontent.com/IHLIlNOUXVgatUmOBNZX9wh0oNahFEf7izpWdBuJfik5A_KFdRFuWMpdfG3iDVccuysqShky8Kw2CdBtrPBS0ZOoNYEIPibwecLoCRlkAMHvamuBeINuWFS5WB61CCrDJ2icVTfP)

Volvemos a verificar que el correo se encuentra en el directorio correspondiente.

![](https://lh5.googleusercontent.com/LVs6ZBXU0iqcG8tV8JORoOb0c0633GXQx_hNfFp_qE5K8fS6dpPGU06X2CU1Qqimdg8icdrPSPNknVfwKIqrYfQlh9LAg994hkM7pdzGZRbW5jHKVE7mpcJCiyUtIaEyIpE4UOOS)

#### POP3, POP3S, IMAP, IMAPS<a name="id32012"></a>

Lo siguiente consiste en instalar y configurar los servicios para la consulta de correo. Comenzamos instalando los paquetes correspondientes.

![](https://lh3.googleusercontent.com/K9JC8-9cREF5YA5ouqCZUMBHqnIWBGT9XEpXDkqfkf4jV0FxcsKNLph1oGNK56yWW6IIZTrqhMuWxWjFpRR5ZhN9q3PMkqDio0I8i1V6_nZh-WIByKHCdBA0qHtZOpxgDXCVChFI)

Posteriormente volvemos a editar el archivo de configuración de Postfix.

![](https://lh3.googleusercontent.com/shINTj-YJ_K0V_1JyQk3K0N5QfA3731ZreO27Y1iVWghhzUeomOZGA2sJm8RX6J3vS7K4YbkEpUojG37xJOos3InwrAqzLWRZEAIcgHoGf5WIpw4NtogkSYh6yRkZanTR8LLQnIx)

Agregamos las siguientes líneas para establecer el método de autenticación y reglas de recepción y filtrado de correo.

![](https://lh3.googleusercontent.com/1yilrVHq7_pZvnCjuMjXBpLVKBARzQtAajqXaRcgoDfb695KBYKju2e9E7RkYy6iTrS5nqej5IP0YdoL5vKEqaNRotPSfRVc9I44Q6vAsJ0nwx56rvpHam8fmKlT-75jM0oDvG6K)

Ahora editamos el archivo /etc/dovecot/dovecot.conf.

![](https://lh4.googleusercontent.com/VzSs5CC-WPYN86o1PN3gAhN-YRlKojO_FC--kmF5nK08Y1JV8AnyoTzKwlKPRLOwfQhYKjxvNlMFd_03BF-st9h5uOsjbfqxHxPbJ4u-R4eCCnkhqWr-LjfNtPwU7ETvwdyTxvug)

Indicamos que sólo utilizaremos el servicio a través de IPv4.

![](https://lh4.googleusercontent.com/xBbjIf7SiZU-XvsBiJjL2pTO52bOlLt95IcW5PV-NJK_lrA6PHbiOAmn-L5sMEeXmAD_fnh6fysA9iSv5KFPp-1P0s5SlQbsGpchDdEOZo2ZMXBQdY2LmKUFs9yxQLtPBhQn2By-)

Editamos el archivo /etc/dovecot/10-auth.conf

![](https://lh4.googleusercontent.com/ZrVHZLbegCdamM3_7DGlkH-ELXJpYRDPRq21d0jvgyFdzB7BC7fl-gtobcVQhU1T6dEtYN2qkIYDYYi-4JqIWtKin2yqjM39ROlOuKnEITo9J_FF9_9cw3HtIdtsD0dOWJuc-Jgs)

Permitimos la autenticación en texto plano.

![](https://lh3.googleusercontent.com/gXA6aNmA6h5BqcDs1i76N4lolRsFKaYrA522zmPYcEO4-6LddZ3d4hh7Ix7GSFn59p_QQ-Dhv4qMZA1QfArBXmdDOqmYIEVARnC_J5UrhKCdVyafHL5Q-OtnpMM4CaOSc8EVUDcT)

Agregamos soporte para la autenticación de texto plano y mediante Login.

![](https://lh4.googleusercontent.com/3Sqj8OikEeO3LzIQ-Pa3vJxLC3ZIZWacSseRRW7KEhQxgPh5fTc6ZiVQfBkXniYwTyF8VlpRcNRzIZByePiYX0v8Ko0nHD8yuuS1swJxP1OGv2ttr1YKcODcv910tHBT59iHcWgB)

Lo siguiente es editar el archivo /etc/dovecot/conf.d/10-mail.conf para indicar el mismo directorio de correo utilizando por cada usuario.

![](https://lh6.googleusercontent.com/7G4l45ONRHwT6bYpDitMHI8ZmsjM9qhxfIVtDYUOoeAF53AQosxFylpZqxuBeYI3ABIUQZmlQfDyHirLrkcXo7gDionpA14fUBGW2pB_Jos5hIG3K6geCXP-K8CinD_HnQdULIJO)

![](https://lh6.googleusercontent.com/u4tiDOk1Yesl1arYmWzSC7n4gjeYY-yoYHb4-oT_sGaa0404__ZHu4IAVjqyAUtJdRqCa2Nrzb0O00OAvKCl9AtwvlD_cUUunfHk3i7mlsco6-f-Lqe0KC8oNbCvvmN7ylbmKNdY)

Ahora se agrega el siguiente contenido en el archivo /etc/dovecot/conf.d/10-master.conf para indicar la autenticación SMTP de Postfix.

![](https://lh5.googleusercontent.com/Hdt6goYAhT40DXhwjI7CpcoTmkVEk-rS_EagNr9HW1rJQolDo6aM5xvJVgvY5UcGOkq0q3jPa2HnzVr8GILcowj1rtGFoypw-WAeaB3GVTInTA-sWvhtgHT_5UBTT9eE1Rm_PaHm)

![](https://lh6.googleusercontent.com/B0WDmy8iFiTCXtIeh2i7kQagaoF7-PV1ob-tRvC2QmEFfU07qQXceOIQdtaHHDUYnLrRlt1zu5HC6t7CuPWWusl0WVOQWps_HuQGNtWxn1Bywns_rg6vJQNi0YU2RkN2p5jtUnNw)

Finalmente modificamos el archivo /etc/dovecot/conf.d/10-ssl.conf  para indicar la ruta de los certificados SSL.

![](https://lh3.googleusercontent.com/ZXkwJKTxD21FqR2caYn-iiAtxtDL5nR20WYPQCAvNizN-bjVsXXdF5OBBI5Vmb0i00rLHCR4IwkLVVueX9kDXf-3i8OLjbJXbXuSU7voiZw08mWcu_oPBYotn551Ktwbp7E_0Ght)

![](https://lh5.googleusercontent.com/ZE8yoeyRzZtxO2VA26-b3v-PqcNbWu2qnn3cUQybs8C3idIEU1Ft9PjIgOQBJ3g0f9d3C66z4RW-QenTpDjgG7iTwZLVlTKOByHII-JSL7BSbkqBpWsRKAQxl5g1F3p3Fulwx9ia)

Reiniciamos los servicios de Postfix y Dovecot.

![](https://lh6.googleusercontent.com/sC5d6pB5JnF3MDes8PeoiC8Qge1e3VxZvcnK6XgCThJRfIuABO2jzhlbnW18BYfEMJRk243bv8g0DxX5N1VIp5tq1B4I8yxnErt4iTrmh-CuTYlrXHh6xjUbeWaGWLc94GUnxFMP)

Podemos probar el funcionamiento del servicio IMAP a través de la consulta de la bandeja de entrada mediante Telnet.

![](https://lh4.googleusercontent.com/nX5pVNsyrx2lx7uQwl4UpHfazrAZWpNKTDzX1Z75tp0gnJ7j2WPWwVrSC5-0Xn04AMGox3q6cWpwI_d3UEc_1xoXFGrPbH2R5LC6n4oHamrE8QIIjzri0ObUD9GQSyv0MFIV7aHS)

De la misma forma, consultamos la bandeja de mensajes a través IMAPS y telnet.

![](https://lh6.googleusercontent.com/xr1w9Zaotqga5a2efvheg8HgV8kPQXzNr9utqmfcaeVsHQGGHZyi3OAFQSFJ8JRBh8Ug-3zysbhgp3Tf2sdgmOmXTULTW_qFKkHxawvyXap1T7i8A5EAh-ERfGoKdaVS93LKZdJZ)

![](https://lh3.googleusercontent.com/sMhYVGqnej-XA9jtLwElQX00oph7shtOpUp6xhWAGdeLjnydc0vPs5mUccXvLk9Zoy5onWqs4IW9bob3ozRCEQwMjYSm8MVUJxIydQGxgFfqEYGAgeOOyNKDXg8tBqvThspFdMFO)

En este caso consultamos el funcionamiento de POP3.

![](https://lh6.googleusercontent.com/0aqOUg2HjV4wWrD1V-vM5SzqFP9DESjLfMprvhVh-hZgeoN03c_1lZxYNtqKsoC4myzUD-Rf5PRs8irhWQ3RFvy8v9qdjuVbI7pNkdGmHrR_99_wmi6rOtttcJ3241dyz-xN5khY)

Y finalmente corroboramos el funcionamiento de POP3S.

![](https://lh5.googleusercontent.com/UIz6v1DXu7Lnhf7aiCOSgdkSn3LxOWgaloYDAkLOr8G42gDs694mynJW-TNURFRIQywMZqytO4Kl0kD5tyH2gai3Rm199DKNPh_OmgRbMfCSZogZY6ZRooKBmyH8uULsP3NT_XgS)

![](https://lh5.googleusercontent.com/K1odgcHKPsWQcp1NeC_meUW0onisk4-uqBzgRZuqsV4IOWEAtdV3ZsRWA-u7AobYRueKP3ziuAdO5LkBBjlPlp4Fj15pQg6n1mJbph-C5shNAMvMs0HXineQLTHIlAP3Xi_aV2I2)

#### Amavis y Spamassassin<a name="id32013"></a>

Instalamos el paquete de amavisd-new para Amavis, el cual es una interfaz entre el MTA y los filtros de mail. Adicionalmente instalamos los paquetes spamassassin y spamc para el filtrado de spam.

![](https://lh4.googleusercontent.com/7X2Lhq2VbIsTcNI48kA8L7FrTqF_VpBW_QQ9qNNABjt262xDOPFKnKLgWjloRZmgTuPB1i_K9DYxybMdXMzFQD_NCUCgsjTQwvQXDDrZAex2yKCJ8v61EDvTLdNLCSjAPwl1jxuG)

De forma opcional podemos instalar los siguientes paquetes para lograr una mejor inspección de archivos adjuntos.

![](https://lh6.googleusercontent.com/syAvjG7lYZLwlx-jSRAIvsDhwtO9AAcUAIb7VAAwCNK74Tjwp3VsmAp6NX1q7hr265ss5XQf3pCybWsX5Z89NBDHb0Ur_TedCETk8r2ESP1Wkaq_zDZPPWphsPj4_5Z1kOjsHGJ-)

Posteriormente integramos el filtrado de Amavis a Postfix a través del archivo de configuración /etc/postfix/main.cf.

![](https://lh5.googleusercontent.com/z0utrINkmivwkL0QbBs2duos7DNCY4v9CB27IL_SLHrDukEJob2xMWCNKT4M-pWfnZHqCL7Z8NFUpBwhfhI0pq17h3hgSOABrPLiBKB2cIMFEFRdNPSaRjivm2CkqhKgVNpjyMH3)

![](https://lh4.googleusercontent.com/OyPQzhNmnbLCs_aEs6M4vzqomwV_z_-Sb-zd5zY3mqZSa4_hvVMryPA0akJS4MeXJCcQZKfmM4k7ww5Sr-lcF66iHjJgWBbwnIybx55FB3tNo4x7bcurd_fD0WRDZPDpzIP9dbNS)

Luego agregamos las siguientes líneas al final del archivo de configuración /etc/postfix/master.cf para habilitar el servicio de Amavis en Postfix.

![](https://lh6.googleusercontent.com/03eL_DUq2Qc76kjfTRprJRPLXszcS0ihNFx1Sz3PH0K7pi8TIQn7aHjgZ3PlixONgrB6lvIYA7dgvHHvcs2Thw6_cnxvQKs7k0kWjt3b1quw_YpMe3z0nYEcQvcmM1HMW_o_ROc-)

![](https://lh6.googleusercontent.com/qtl5AUPzisvPmZnawzvK5NLf5IKIgQNddc7s7H0hxkI9A0c8G-Efba4LxRR8Csyw6eiVdzDGP2vQaVo5kNBaZmJHnxnWAW4xoqxVrHGss4O32ns5b9Kk_m2AUWilxzH17PojBTT8)

Lo siguiente es agregar las siguientes líneas en el archivo /etc/default/spamassassin  para habilitar el inicio automático del sistema y la actualización de reglas.

![](https://lh3.googleusercontent.com/vEFOhXhyG1tJLcgwrjbFbQr_WLyulD_2ybih5TR2MG2cgm05__Zlcb9pfy4I91TQbGWMw2LM2QU0AU9V9NM75r_JgOY2aDUi1OtCLQB0MDE22DtdGOBCg6Xnl_M4Z7c3ajI8jYyB)

![](https://lh5.googleusercontent.com/ZSU03eS-R0itHqO10RG4oQjh1zvdhiJzDZO6MiX6w325P5TEOtham_kDTA3qut-hXqJtokCIXh4p8msvILj58g_w6Ybttgw5O3315lmjf1hu8L0FJFr5p7F-S0xUiovrxGMCNfbN)

Luego deberemos activar el filtro de spam en Amavis a través del archivo /etc/amavis/conf.d/15-content_filter_mode. Debemos remover los comentarios de las siguientes líneas.

![](https://lh4.googleusercontent.com/xPwPS8fg_Im7ZV-6nZrnOSOGFQF6ynBLJsWuzRfM_nrQVgi2pUv9f-emw9pZGfaN6TyHbvWqyAegg7j68jwv0FuXb54tlJSUB8tqF6GnK5v_gGL-Q9nZO6hYDW0z6UdHy7hpUyzX)

![](https://lh3.googleusercontent.com/R3jkAuheYeovUcGHu3fRbzIuWIpKkQsuN0UFsc7OPGjCItjiOVRQPyOfRuICfX6RoLJWQgqBFDPjKGm5RDqujMQlNAIZAPgvuWjL3JXPO-JI6SIjKh3kzV-v2mtmRixqymtXW3Zg)

Ahora agregamos el siguiente contenido al archivo /etc/amavis/conf.d/50-user para indicar el tag utilizado en los correos identificados como spam, el puntaje mínimo para agregar los headers de Spamassassin, el puntaje mínimo para considerar un correo como spam, así como los dominios a los cuales agregar los headers; en este caso se agregan en todos.

![](https://lh3.googleusercontent.com/tGIa2yx5Zq0QOBpMcB_KofwTBianNobnLes1GHP1NNex_q0_nHzufSHq5CVZAUVHjK8CkvshD9gNHIjXroEI2GFs7vU_Vwb47HKWeWGuEWP8_uX1eb7zUYbHo-SzhTGyJjrryIU6)

Posteriormente reiniciamos los servicios.

![](https://lh3.googleusercontent.com/Xujn5-VL9-Wf6ZnQQCgiW6SWSqvw8HsqaXoK1WMJwz699jZzyX53otp7aJAh5vRlTEhkJb-FjOE9Jv7M1b_JBGeRmkvF91Gpvj1b_GbDyIaDda4QuVNZdUsyZf-E4rJuW90OvM6L)

Finalmente podemos probar el funcionamiento de Amavis y Spamassassin.

![](https://lh3.googleusercontent.com/VrjleBZX0zkNTUTMiCQyr3PcVc77sO3eJLk4ZdSBWFDYcvtcCvy0_aUPDpFb1ZWT_sHOvhDLP_HpPYqd-D9_3SzAieLKMOwEQK90uqbU6SpZ8Zzvyb8CqF0IqX7VHOlnpGoGpYii)

Verificamos que el correo se encuentra en el directorio del usuario y que ha sido procesado por Spamassassin.

![](https://lh6.googleusercontent.com/CeKAuPfsX68S9T15FgDsf4KQBOTlyMYcyjhuB8BZoI2g1QgXNFqcpO27Cl7wLsstdU2WuU2eecKo9iRNGwMKZRPwUBVucmG3MkBDUG0DJO6-I5z_RavKI8TTSWy2YbCtkzaYIOhs)

Como se puede observar, el correo contiene las cabeceras generadas por Spamassassin y en las cuales se indica el puntaje obtenido, así como una bandera que indica si el correo se considera como spam o no.

### LDAP<a name="id3202"></a>

Debemos instalar los paquetes necesarios para tener el servidor LDAP. Estos se instalan con la siguiente línea:

`apt -y install slapd ldap-utils ldapscripts`

Durante la instalación, se le solicitará que se configure la contraseña de administrador LDAP

![](https://lh4.googleusercontent.com/ACQ3oWXK1PYzux7pLrr13vm-gqXqTbl7djF5UvdXoZ-iIZkHu73_OXmKuCeOOLyf9ZckeoNj-aW1GQoc0SCXj6fNxn98iPhmg3qmsbvqti3yyGB0LdZ-D07Pet4EOCARWYJnG_8I)

Ahora debemos configurar el demonio slapd para añadir los datos pertinentes a la base de datos de LDAP. Así que usamos

`dpkg-reconfigure slapd`

Cuando se ejecuta el comando, se pregunta si debe omitir la configuración del servidor OpenLDAP. Seleccionamos “No” para que se cree la configuración.

![](https://lh4.googleusercontent.com/l6f8hgEG3x3Y5KC_jyDXql4ly8KtSHgOlvhcz_W7rPEFSx9dt4hluXR8JurE3cCxRhDmyhzSpDHXYkC5JpnEX06qAqb4xKdjMwhsGIMq2L63dKfRJ4JsyrDeeOib6zrmCWbh2F0U)

A continuación, configuramos el nombre de dominio completo del servidor OpenLDAP que se utilizará para crear el DN base.

![](https://lh6.googleusercontent.com/FEuKKzyz-8yqfrQdQuRAG3-CbDRCZZShyMIZeMKHPN-ej1kjH0RYDhcmL526HA2oK0KwRcSLp9LONifpOi_zxdAaikEe3QepDLfdzsOQAenJPdeuEzzxubILIb0_cHH_QgBrdjOC)

Establecemos el nombre de la organización

![](https://lh4.googleusercontent.com/rJZXUM8FWzGKnnYBIKyMQ9zbeC7brw941U2FtHiU_1tWrcvMBRnEAKEdR1qFl_Kt-HkljpEs1mFwaIiUlwger7jclcyH6P5MkZvFu-J9l2Ru1tP4UsGqlVNJmqWRMM732KuC8Ud5)

Configuramos y verificamos el password de administrador

![](https://lh6.googleusercontent.com/wpusL65v01scw6eikxGiIZkajSwLeCeqblkCVRYpYHCxJwJldto9oD6bVHh4fWfi3BDb8erN81PeRyph165wDQwDmQI8h2IhVJ4XVS8LYSLWP2zaJpq623qEV9d75dzPPzYRWZxJ)

Seleccionamos el tipo de base de la base de datos OpenLDAP. MDB es el tipo recomendado

![](https://lh6.googleusercontent.com/e0A_uAg9xFh3R0nfLduEVAcrOg3-4TLfat1TjrggY7f5uzFTcCoZsi01xUrF6fh2wkkIiU2ZdsSCupy8_Ekyamp_-msE5V9La4h8kbOehV3NBVhdhgqrcE2mh8wGPJduyhPHu9ET)

Removemos alguna base existente

![](https://lh6.googleusercontent.com/APMaJgEXOMu450ZtXnLDdadHqHnC-I16U36eWdnZMGSxuPfD8SqK2WHqWTDJ40DIPFAKYuTvS3wqN7fa_G_spPNRutoObNIICn6TwGtI7zP_3wiRvbfYB-Z-WPoc2lEfCtfYWMMb)

Aceptamos la opción para evitar problemas en la configuración

![](https://lh3.googleusercontent.com/Ba2RC689Q6wXxmikhy_k0R7lEam0ilXb9AC72UW-9n5ZCJQGGhk8jdUsOMHn4pe_LTJtboVlcEoll5H3kTM-8Z8fA-PrWRmLdjEdwjqJ508jkX7ACVRKXlibw4wraXN1TsOP5rbd)

Ahora que tenemos el servidor ldap configurado, podemos verificar los datos con slapcat

![](https://lh6.googleusercontent.com/j__Bs7A53k5FKNGterXHK0yPczeIJEMEVACM5IvDoP23ZiUOBXmwO6lloKxX2YnDS7woUohDCY42J6U5sg66mZM3r3zwKMVUtrTjiwJVnZNePPjOvArx5iICO7N50T5nLgd_VMBX)

En este punto ya se tiene un servidor LDAP funcional.

### LDAPS.<a name="id3203"></a>

Para configurar el servidor OpenLDAP con certificado SSL / TLS, necesita un certificado CA, un certificado de servidor y un archivo de clave de certificado de servidor. Creamos directorios para almacenar dichos certificados.

`mkdir -p /etc/ssl/openldap/{private,certs,newcerts}`

Una vez creados los directorios anteriores, editamos el archivo de configuración /usr/lib/ssl/openssl.cnf y configuramos el directorio para almacenar certificados y claves SSL / TLS en la sección [CA default].

![](https://lh3.googleusercontent.com/cYoHQqfXsZnYGixZC5Erinl5qUdjLPqQlmHRbPpngvIavBgk4kisRDH_lUvaXvDVyIh4Ekn2_nIyf6-lFVJtpZodbD_pRcWCd9zheGWqz7gcXkJRnwYSSwFLXzQ0Rn7M8JDXIFvn)

Podemos notar en este archivo que se especifican rutas donde se guardará el index de la base de datos de los certificados así como otro archivo para el serial. Debemos crear esos archivos
```
echo ”1001” > /etc/ssl/openldap/serial
touch /etc/ssl/openldap/index.txt
```
Una vez tenemos los archivos y certificados necesarios para LDAPS, procederemos a crear un archivo .ldif donde indicaremos la ruta de estos. Este archivo lo llamaremos ldap-tls.ldif

  

![](https://lh3.googleusercontent.com/zWYJtKfit-UW43sf5689OzlN-KnO2M_eWrTRkDUlgZ4J_LcADvwc9ZDSpZDF-jOBt5V1uAHSbrby5hTyVU-9LnKlutyZT2OeJ4QlgAoSZU5mFVMzM0dlF_aCeIFIjRGa69sxE0AI)

Y añadimos esto a LDAP

`ldapmodify -Y EXTERNAL -H ldapi:/// -f ldap-tls.ldif`

Por último editamos el archivo de configuración /etc/ldap/ldap.conf para cambiar la ubicación del certificado CA y añadir una línea que permite este tipo de comunicación

![](https://lh4.googleusercontent.com/_cNXNpRoS9AP5Rqmwj1icGrVjsTlvhUuUovrHPDF-s2TMnbGtWbqW3QI4YU8BQQ-2RUgVTco8q9YiHC1Ng5JJrsNjFAQqSj_IE6Fohv7nDlwWFIJ_mMs6DaWKfCxCTtOsJneYAAc)

Reiniciamos el servicio y el servidor LDAPS ya está listo

`systemctl restart slapd`

#### Habilitar logs

Para habilitar los logs del servidor ldap, debemos crear un archivo .ldif (al cual nosotros llamamos “logs.ldif”)con el siguiente contenido, el cual habilitará un nivel de logeo de estadísticas de conexiones / operaciones / resultados

![](https://lh3.googleusercontent.com/M3j4yElqS4ruAhpn275K6JZkevp5KDV80ve6Kl84URfZJpnMd1P9JsBavJ_XdW417E6S3EhaIIXpDBL5xCWoFdW5cR0ZC46KbJ-biwKqv2ywd2fPOC0eS09Kzts_reIN_2hdcc4r)

Posteriormente lo añadiremos a ldap con el siguiente comando y reiniciamos el servicio

`ldapmodify -Y EXTERNAL -H ldapi:/// -f logs.ldif`

Ahora los logs de ldap están siendo creados y mandados a /var/log/syslog

![](https://lh4.googleusercontent.com/Dmbij7ZhW39ZJDasAdIE_mZSaxTd_xrxZ8xSm_mPKYDyuDV1LCJbkRn8zrHhoYmWCuRNGdIbXAIJ5HT6ds-awAoQAuQGLImSdLmAUsC1O9-kkesWF_6NQB7Q1yjMpVpSKIXQbU3C)

Para finalizar, deshabilitamos la autenticación anónima por medio de otro archivo .ldif que deberá tener el siguiente contenido

![](https://lh4.googleusercontent.com/4AE_zIXegfkhLX1wfTQanK6hMadzdQmc4IfPIlU3FmllLy-6yczdy4x8NR746cylytTfolqh3ctJ0BYFIDjNLEXhkSkq73K2v1BZ0rYZWmLaZjXBueIP2ycoP6NNgGVDtKxkr8Zf)

Lo cargamos a ldap con el comando usado para cargar el archivo logs.ldif,solo que hay que adaptar el nombre del archivo que se carga.

  

### Apache HTTPD (HTTP, HTTPS).<a name="id3204"></a>

Servidor Web (Apache NGINX)

Creación de usuarios para los servidores

![](https://lh4.googleusercontent.com/F9tICsHZws1xqHVqmphxw9L4wDiZC5jRT2D9oAFtT1E8vJDa0axSRD0AVQp1bu8UKQNZ9fV8rDun9zWbkdewJs5rJuRwEeKxE6QS4CoO1f2YwnHNHgWIih5KCMWFXLt-j7cBkyJY)

Le damos permisos a este usuario para la instalación de los servicios.

![](https://lh3.googleusercontent.com/IXTViyqMznr_Pz32OdPF72TFhb16Gwh0sT_qo0XOwPTvt35nyu5Yg0pRersnGm0Km5wngfDyZDV56rs4JRR0wsAVLTmnYbx_znqeSUWPKOLXyVZ8W6B0SOs_xDtlYz05YRpDb4ZV)

Configuramos la IP estática

![](https://lh5.googleusercontent.com/ofRqI5sZPZvjngo2D0Llo7j0tgU3H9Su9Wp6dVNiC3jsIl0ylPgEdQzwpwOxsM5p04D0j2yNmRLCRfvbzUFCb2fhfbFlHi9WLgYiCtcj2A067PdEejd2ZFoZXVRkhVvOkQXecLfI)

Reiniciamos la máquina y revisamos que la información de red sea la que se agregó al archivo.

![](https://lh5.googleusercontent.com/g7gXkdxhuteOyCPTESL93tWml89-Xq99P3mZZc3_JKKrqhu1MME3_YVJnmArqUIaXhjjnaDBhe2_M9UlroFl3kwG9B5__SYFz9rOsR4oKv_DtzHnee7QuneuozVR8R4WNDA9UbLZ)

Se ejecuta el comando sudo apt-get update

![](https://lh5.googleusercontent.com/ZoD2no-pflYSAHOb_5gBVAK5sIvYeWQvaxImqVMd_7tP3RYHt2i22QyEiyOStas41NnNLMyBIBgkWKT9WIRqhXGoY-SdTOayW73h0OUU5GwashVy0JaGT5CpQLTKjN3wag7FuCCf)

Se ejecuta el comando para la instalación de los paquetes para ssh tanto servidor como cliente.

![](https://lh6.googleusercontent.com/gonlRd_wm8ixA6HzeiccrOnIF8K7H96jJ4IBwz1qgclidYTQJ9TMNzyV0_R6TyCsnAfjXHe6GhUTgeDUAbLRWtn5-NaciiXLsSTgHNfzJSyfbSzhWjtYKAMtFrNJJtyCIfqxN_tl)

Verificamos que el puerto 22 está en escucha.

![](https://lh4.googleusercontent.com/yr164NEi40Y0xF30PcdMiqCCA9l0dzqrm8Tc_W03gcRqjN7SC0L6_VHvj2piA3fv7yBfeklpboLpqqt_F_oXORAqjcZ_0YmM1kIAF-Ur2vUGf5mnI4IRuJeO6zyjpB3Sd_S2P0uZ)

Ejecutamos el comando sudo apt-get install apache2 para la instalación del servicio.

![](https://lh3.googleusercontent.com/2fAp19LXpPGFUpHbU0vUZvQ-0wzfWB4kBwpqTvdhHKjYHQV2jL8-LuHkF3f_4ts3Gpl8WUh0EC_WMTYCJe1qKsr8DZdzb16ULrRAkHgR1AeSAiwfjekUeQHn6HChRE7YwHZ1r7eh)

Revisamos el estado del servicio, para corroborar que este se está ejecutando de manera correcta.

![](https://lh4.googleusercontent.com/JtCtyoUyHZlzKSayiyVRMoI3BqKeminYYZ8Jc6gE-XT79K1WsBSnmh43jhdiYE1FtPygtmROQiNv08PWGX3izZqCFJ0rI852v0Y3O_ZqF9OqQpiIMSKp72F_QIAbtQ66FHvzeKIc)

Habilitamos el inicio automático de apache..

![](https://lh5.googleusercontent.com/8yjFjQsO-1sLAQOPe8Z12wh0C_GYj7udKGeY6x7k2Q-FszWlHf2xzBOrnEXGue5Ejdj4d--OCQivgGPativqUJR_oM-06DtGXBp4o10sVdg4_j9hCoqT4o02JW-FUWfWJPbcYcEi)

Abrimos el navegador en un cliente y revisamos que responda el servicio.

![](https://lh5.googleusercontent.com/b0MnD9fulcBfBxBcYT24wq4feDOko6drtZrIMv8CgeQTW4E2iyZYmca02TiB2izHlV7N960xlfSErID1ah871G-Ry_yOLMDF8XHbCrrT9q0D2VNaIIayUTK10-KpVpwCATuy4IGW)

Descargamos los certificados Wildcard. Posteriormente se pasan a la máquina, en este caso se hace uso de scp.

![](https://lh5.googleusercontent.com/PUnWDqn2CJCZwNgD4881Vw3ypVidHo90b5DtggxDj0pankmdOgg761VeyVg0qGbeTqWrkJiei1gNuOp6obrPLxFA_5csqU1ufscxpgDaoKbEYnAtkI85v95AXnQ03ZJ8n-GxvPQm)

Comprobamos que el archivo está en el servidor web.

![](https://lh6.googleusercontent.com/tKFJHvKNzNMKGSTmukDsJFgja8ZExehFW8jyYnwxadIZ5MAo8T8fYJIWe14dbKeucJ563PgpjSh-X5YIcg23c6E1cwKyHmuF3oJiKer6pdnFaQUyfmxLw21q8ffkpx6szxe6ei7i)

Descomprimimos el archivo y revisamos que se ha creado la nueva carpeta

![](https://lh3.googleusercontent.com/OD6AgEq4lwOC53AO2AMutyRlvD5FwsiWNT_iM7pibKXKCiPeUC46nJ1lSo0Hj2xQp2e0CmYVAfV8i3CKEZIUDolg8pff7FTTDj9tfz_8mnUidAS0IrPKEb7xi2zWx9_NynNhGNEA)

Copiamos los archivos a sus carpetas respectivas.

![](https://lh5.googleusercontent.com/vq_0IUTCmuoQGjmzSQsYrT2414K9vThM2QhznxlapNPJDgYKrLqTZg5FeTMlGAbxT26GHh-PjJDiLLONCr57CaBnz8QY7s2rs84IarA8s23sRUAEJXtquZXA_Off7Dr3tBMrfOr1)

Editamos el archivo de configuración del sitio, el cual se encuentra en la ruta /etc/apache2/sites-available/default-ssl.conf

![](https://lh3.googleusercontent.com/0UMvsJozfn9YRUUN57Rok9BQsemw4KaOCHTZcxK2pVWMuxC8D_1XslF-ehdOQ9xKedmTt9e24xWMdqJyFG5liBte5ajIBlPAFPOXQM2f52C27dZzfo-xZP4d2R214_4GISQsHrn1)

Creamos la carpeta correspondiente y agregamos un archivo index.html, a éste le hacemos un cambio para notar la diferencia entre ambas páginas

![](https://lh3.googleusercontent.com/w6gmPCU6pdr6pKUk4nmdTXLXojoOsZqwNk5qALdb0MMlrJPp5ZteJLokWzyjTeJQYcFJvWY4mNfup-Ew0hW2nc3j8zT44Me9BODni0bPzaA8_1v9Bg8bMKF7D9DcSuxYIm7m58nX)

![](https://lh4.googleusercontent.com/uIIkC87rz2W8EOxwp9IBvL1Au7_pjQzEyBkv-QVOrmzJS2EGp6_usea8d_Q1LlnUvOyuOaPkqfMbD_okNic_PI3drRLX3JSrDSA6lYltLdktk79JgtTSGldVV7_MMc9H02ogcPKY)

Habilitamos el módulo correspondiente.

![](https://lh4.googleusercontent.com/qjEoBTxhUSr_5jrbZaQAoF9sN-o9kJwPkv497ZGuUoQZPdtbtr1szo2Os4apJRbJ1vX8PDN7wH5bVyXNfmaVbDPNUbgAPpziLzdSKHOk5s9I5R20ySXsumGccOOoGHUGmOyJTIGj)

Habilitamos el sitio web

![](https://lh4.googleusercontent.com/HD4ohlAArF6isEzj_feObsrbx5t9190Trx8V3gOTUXE1ofVgc7QXATB865v3HJ0odnZuoK4byxU5alGQcjFE8kVN85I1FdJJxIhMUMbP_xwrbm2mup0IzvwMG4oS19pss9dLkv7R)

Reiniciamos y revisamos el estatus del servicio

![](https://lh6.googleusercontent.com/QykurqzhptlrIEZ_Uw85iZavSjrtqbdMGV7a4l2XBBV_zEG6rUuZu4-Vca4jj9i1pZ1h6jCULbylsaw-JRd2TIvLUb6rS9t5lyV8HqqoOSuoS2Mo22QBjh7gq18QhtRVC7ptG0aU)

Ahora en el navegador visitamos la página, con el fin de revisar si esta responde con https.

![](https://lh3.googleusercontent.com/e7TNiUkoLwRkENE4dj3CloR7EblviX_chSsYJSSGrnpjRwgTpWZwet9vkMGFXzeVRFhXXhlcYWxqXYYDSgZolZCDXuSJYcHZNZKA-ftJyOoiGSb3vnPwbRGP6R8FUH93sdbDVqTZ)

Damos click en Configuracion avanzada y, posteriormente, a Continuar a 10.10.10.9

![](https://lh6.googleusercontent.com/P-ti41MufNSLZ5cO-BOlIT9NvZOle2i6JDA4xyJ3E4MzMZtEfQmpu3NILFaBTWfBws5tsb6UyjCR7G7OOgapGQbLRftsyjYuvacf_yR4e3_SedR_Wf2uwUy71RQfyOJnAkvaciWm)

![](https://lh5.googleusercontent.com/K7MIQJMcXcJuucZkKMMcNucXycKep1AQOfNKnrzsybFuHWSt7G6yFCs_OadBX5MOBO8y9Fjqa0URB42kJxFQiAOPRsxoj7Oql3Wszd0JPuPM5qdEQxB8nD_y35ExiY2t1fJfuVrE)

Configuración de nombres de dominio, para este caso, editamos los archivos de configuración, donde la Directiva ServerName es editada por el dominio al que responderá la página.

![](https://lh5.googleusercontent.com/kL6rTs-6trPsifVCDFAIjae5HdDInM7IL5H1-JEfzveNU3I__8_j61Mjh1xYAWwB8uyyPCn_IDu1HfEYKE6LXHZQSDEuxT1gGfkYdRxXSyHHpbRMq0GMpo7v9zaZI60Nqbh42x6e)

![](https://lh3.googleusercontent.com/3gGAi5IgW7-FuYhDfPg_G9LWgxxHoCS0OiV4NyJHZnU0HXWhLhn7W-Eh7Jaw9VEEoAL1EfvuRsgt_Op2BSLhBTRpVHK9dHGb0zoDF6Jv21B-kSx1RDgNFaoSPN0x5JohEu0cXpeo)

Deshabilitamos y habilitamos ambas páginas, además reiniciamos el servicio de apache.

![](https://lh4.googleusercontent.com/mJ89F1cW-K9v6gXWbSOaIKumZ-932ikfwCFmG5KZPVq1qv6ah96zqDemFb_C79mc135YJt4-SGppg3FtglF3YTlkZv0X8iKCAN1E2zDkywh67BGTW_Q1atEdsf39BS4_kQ_r-NOO)

Ahora se pueden visitar ambos dominios

![](https://lh4.googleusercontent.com/lYDi056MoY1_uqbiOjAipBGM7BbkSCR3NuUGHTZAS4QziwQw6WcWXiyr_Bv6vWml7UF6lLBqE_Zcm9zYmw5LtsQDvzGmguOFNSwWiYcBIUWMhz2lNN-9Hujbw0d5dgEpHHMdIYSK)

![](https://lh3.googleusercontent.com/0ooswsCOkcSKY-x2RLc5qC-jyTUfko-Siu82LiP2DK6aChvHxhdyA-UpFEw4rthffcXUlHCN9EKTEj6DsAUiqGGZFJML-_u3B9xRyiRx_nW4mKpg0jMRJAK8RH-Yqb7t2XDoU81u)

Para que nginx funcione cambiamos los puertos de apache, para esto editamos el archivo de los virtual host, de manera que http responde por el puerto 8080

![](https://lh6.googleusercontent.com/HCIId63YFOwaEAejHVFBqN84WvPq1ZG1pX-kxExBLg2ZuKH5A8cDjIKFdloG1blTzj4eUsVeT8UHG8eLxo1X6rONpXW3e_oFLRmdlbx73MEjPdCz4g1L2Trh3iQgWyI8KDdvPyLh)

Mientras que https responde por 4433

![](https://lh5.googleusercontent.com/McZ0u7l5rt72vPny0miLLWn0NQgab0Wrb8zv2Q5iNohWcVBj6ll6qvg7FKnsGQyDKWJX9ONx9oaMY6YfOoIrys18r-J38lS8_dPp0dzcPjGjxB8KaVGmeZMYhNILzAVnkszaGci0)

También editamos el archivo /etc/apache2/port.conf, donde se especifica que está en escucha por el puerto 8080 y si el modulo ssl esta activo, por el 4433.

![](https://lh6.googleusercontent.com/1hj4I-t4n2CAfMRm88JCyvNmBRErdyoLtV4dRxxwMfuyK2bbydGy0c6WnYiTuzoF-DO8keGREaJ7rcAuTAYtzTTinq-QFNxs1MfFV37gn14RtTOW_S3feilVPRX7gcrYCS8-QajL)

Finalmente se hace la petición a la página, especificando el puerto.

![](https://lh6.googleusercontent.com/eqVHpY97P3SeDF8xHlhK00AHaMt1h9w43M4GqdTKJL5rtS6avpZnnovtcRY2ulMmzVz4L9HJbTCQm1aZ-eMtm2LUMGCL2FE1s94uJgBkR1sehS4pMnq_-MUWDChFy8B_zfhM9mRj)

![](https://lh5.googleusercontent.com/yRBKsOh7zyBJ7R7tL9KeIWmVl9IB3nzWVCiF4cSy7nXYS0H0yEqaE1RbGx9G-2s1aYXMIuaYpu9L-HKAEJi-0AjmLe7NR1AcRw7D6AyJOPS4H625ZUoIdsbwxriY4jgXMGptTW5q)

### NginX (HTTP, HTTPS).<a name="id3205"></a>

Instalación de NGINX

Ejecutamos el comando sudo apt-get install nginx

![](https://lh5.googleusercontent.com/-7vlGR1MHYbl4uur4ukQb5oFhld6HvoIwDBCQD3VHkuXSgjc77UhBJpHw9LYGUZiSBxOHvPHP-I3sxeGr2CG5-VoxfV3RrjZVvu0aZfzw02GKjnKYougN5sG3ng6M8xNMcLyGMLR)

Se procede a la configuración de los bloques del servidor, similares a los virtual hosts de apache. Para esto, inicialmente se crea una carpeta en /var/www dedicada a nginx.

![](https://lh3.googleusercontent.com/QKc89-s6-IlZHUsAjmpQxYAQIIkmNvNkCNL92biuH1UFlkjfqv3tHWXOqGvGIFoJwfD9Fdtij7k_JyHVOD5kg1me7glBdI_wYckt0CXb9qh2FGcpVBIK_7JMBM58XMy-lIt-y5xC)

Copiamos el archivo index.html que se tiene para la página en http.

![](https://lh3.googleusercontent.com/El4gULxMF0DdSzMhFO37QI50nU6678qM539JOX1HTq1UJVVv6h_1HNCJTxhX-JI_GNeDHkuXM4SnXoALQm1SyV86cbbExSvfgCTgdW2UP_4WZP5DWWgYJWtT_UgHXO6FIX1zZH_Y)

Modificamos este archivo para diferenciarlo de la página http.

![](https://lh3.googleusercontent.com/GPDGy5vGAn48Fa13KIsQ76pDwKc-sb_msyH-FwUMpPGeOGNYr5rfSF0npPIsr9stbpLRwgPfHtgjuga6o14atTxh49nZGH0LBJCpwayvwhUt8r1dQdnWVeFgr1gQ89c8Ogh0N97M)

Creamos el archivo html para http

![](https://lh5.googleusercontent.com/_APAS0OGU-4C7nGy2Q0gRzKXbz0KC8tz9a5ew0JRPnQcpx77K2Yq1ngXH6_jBN9pgSOvIWyd80W8XbOqj9-k2h3Oq3JB4Wx62FGa4uRrX3x_2TuoMzl5j_KYVh2WKRByAytiHand)

Creamos el archivo para la configuración del sitio web.

![](https://lh4.googleusercontent.com/GjPmlKF1kdg26XiapvC-gJqgnYemgvv44a4KsX3FPFy-WMbV6Ai8vp3Bbw2MXphW9-Bldo6qs5GlXB7JvAhkqaoCcX98x5aqEqSEX3u_tAccnCF-KWSgEe1yjrTjTAwLsC59-JpU)

Habilitamos el sitio web creando un link simbólico al archivo de configuración del sitio.

![](https://lh5.googleusercontent.com/7Q5QEkaYgvHAF7uSt99xs1549Fs--keLz9qX9MrTJjyvgElKDKz8HW1hKKJKz4lp9lNYD-e8bBZLJ8PN_uy0tBzdR6oeZbHwBKtYaEMAylAeaMLc5fnqYqcW-szw4hbEXOAywu63)

Para evitar problemas posteriores se descomenta la línea server_names_hash_bucket_size en el archivo de configuración de nginx.conf

![](https://lh5.googleusercontent.com/EPEExVmEO0OXusOSNjvLMl9ec_jf02F9td-qxoh1_JJ61-vI68vNGtmsuve6Oems-x_fw6GUctrnRcafhCPpVoqIzc2lASSk3NmJ6AMNUkk_bTbF3YCSMUEdUZ3Z9QAUytcdPLvm)

Comprobamos que no existen errores en los archivos de configuración

![](https://lh5.googleusercontent.com/h47lEq3rSjSogMQ64iAtpaVZImcVSNL2hPEkE5L7SlIp-Iac9IJZbvV_G0YCCQKFoXTCr8H2PbnTTVUGzn1_cASWZcAchkTq7xzF03EjX8ATd_TubROjH496GBx88Twhz6YFiCd5)

Creamos un archivo que apunte a los certificados obtenidos para la página https de apache2 pues estos son wildcard.

![](https://lh4.googleusercontent.com/fuN6FeGA1gBmzkNUaKsSctnOSuenpjWKC-3C1ilOqbT4eSKRW7DlYGJgftooMgSjedvzqSjcnHAJVSfAmDf7TyCdCm5BWrhlkarIYxDH0y84I2ZZMHhkIYfVpf2_C5u-G50PeZoe)

Creamos un archivo que define la configuración de SSL.

![](https://lh3.googleusercontent.com/9mSMKSKEJGG_r0PYRT00AdxvU59ZfuKEBkN4aHC98l0CJT1qVE996TPIe42Xe_RUwoNVCBlZfZQtlHFk1a2cEsmgomtR0ivL5r8qwmtdiptotdMy0C6m80Y-rAEYQyXl23D6IOkW)

Creamos un archivo de configuración del sitio web, para esto se copia el archivo de configuración que se tiene por default y se modifica.

![](https://lh3.googleusercontent.com/gu9qY1Kvq5-C_NoNqBrF-ViIEhZkVpv_ojt_SVwsjSwbzmNnxSmysK4P5lnBuT9pTiLTausOaiemSp53DNs2_RMeR8FLernIn8p4G4BXhB5CxJ18B26Ky0H3yPLBxcOq5X9y3RvO)

Creamos el link simbólico de la página para los sitios habilitados.

![](https://lh5.googleusercontent.com/iLaQ0YUZcYZan73EqxiiQsUkL0wfRTwiV1VlqAuscJ2FHuYJTmAgD6vr6zDh8eVw6L_W1qcOYRxJp1S6u_iGdnZER4nB2MDBJIUJ0KLmc9AIwH2h3C9ECAYa6eyA0qckmGM02Mzd)

Revisamos que toda la configuración de nginx sea correcta

![](https://lh4.googleusercontent.com/PxoodKTN2ifktxIdVzGlCOX5BqJNNmwJcXE4cHmXsi5e0vDO-_cs5s1DJgNzxnttrxyv98csn59H3q5c3liQ3Db23JXM2BCwGLbNq4tdj2MUwkWxrbdKM_Htld8PoNkn7Gf2pA5U)

Reiniciamos el servicio nginx y revisamos el estatus de este.

![](https://lh4.googleusercontent.com/DmOnpU6CHQQ-6NO6Jc8RdDlEsLmRO4LiHdlSqpnDGKWKF7zNy-hajO3bdR1620en74ZaQrBWFdcyOw_3csZVPRsfWdoekoArd6va3k-vTKhVlHLwFBUZ0aI-MNJYgaKo6fC2DSej)

Ahora abrimos el navegador para visitar la página, donde se da click en configuración avanzada.

![](https://lh3.googleusercontent.com/grLDtqlT-LS58NvbVFFN4Bdrinc2cuBHgs4wPK60mOG8lMuM0a7k34zMf2xmz7gW-jblIbKREJuj8lEhktYnuj4WP8PQkPfmV15EVuKIFpsydaCOev_TstJeh3zexoEKo4bMjJjC)

Donde se da click en continuar a 10.10.10.9

![](https://lh4.googleusercontent.com/x-cdprglQ1cSteKNhRvcujOlPTTo3l6EewyZquhz1IoRr4sdWCfnyXkV2EdFN50tlzxkZRASSNgxyAVMuybee03kjDo7DJ5FPQwY-Yrbr3eeCoPB3Cqt6t5tCnF6yl5YWb1trQrf)

![](https://lh4.googleusercontent.com/JNpqZCgS9zv_a6mVZ08y8qFpUYxq5uR2RI4c866fSG4bTOWrdaYx5sRVXTbEdYEcNUCpSd_QolomPYWmPdtmFy_u1BjPhvuDKqyW7W6T3CkOyR5-hEO58ceGmw3r3LYrbSwG4Wzd)

### SSH<a name="id3206"></a>

Debemos ingresar el comando

`sudo apt install openssh-server -y`

![](https://lh4.googleusercontent.com/SKdhZfTp5HEF8VRpopyWuIDMxb59XSSP7qDl-5V1FHSTDpF7wR4frnXp25b_DaJqN5T1Hb_xe48Q0JFSs0zeTmZOeJOpTCdgdjYhhK6jILbghyVnOifdPXdytSLFbL65Ipq-m219)

Notemos que el servicio ahora está instalado y funcionando

`systemctl status sshd`

![](https://lh6.googleusercontent.com/yIJ_aVfpfAXQ93woDI1OmhfbdH0JbQ1UTw7CzrFOMy7UFSvql0POqbqh9PF55W0zPRrEv-rvK8PClGcRV-zBIvGQYUPMKBwkvV62MhDMVwfNQ_6O9b7ZeTLRKVJ0DbUUK0ArwnZC)

En este punto ya tenemos el servicio funcional,pero para generar los logs necesarios que necesita el proyecto, debemos hacer un pequeño ajuste al archivo de configuración /etc/ssh/sshd_config.

En la sección de Logging, descomentamos la linea SyslogFacility AUTH y comentamos SyslogFacility AUTHPRIV. También debemos cambiar el LogLevel a VERBOSE

![](https://lh5.googleusercontent.com/2fWKxhDR4AC2BlXWhPE37wErlIMhTpwPMrOhvzA3bV_ps0b-YaeF0L3FfQUAg1RbewPlkxDd17S9d5-zUR2CrUWzkO6UvcvlJjTmc9J2nfBY2g46hX6xtf7h3qRDUe6PTKBwFm42)

Ahora los logs estarán llegando a /var/log/messages

  

### Firewall (Netfilter, fail2ban).<a name="id3207"></a>

Creamos el usuario para la administración del firewall

![](https://lh4.googleusercontent.com/j8-KlR06Uldf1iA2ZmbMr-lIH9bYU7wKjCLVuG6y3XjcTgZMq31sq2TqwZSbA9TjcvjzYYdASd8Lo6lweYSyzG1Sp-OPG8brXvKPZOLWpaPr5ZsUdnk_mkl-CrQDBaEL4sqezJge)

  
  
  
  
  
  

Le damos permisos al usuario firewalladmin

![](https://lh3.googleusercontent.com/d3gGbCHxGmydZFEdMzbwKwOHP8RPpa3t_pm-6dUR4X8Q0AGaGicMgxcbKSlzFzWHCDQAZAJ8gvhOkc6IvowYuJQm2V4gIrJuOBRSJ9I9EevUV_z6wR07NU4Afhfukpg1mi8LHVm1)

Cambiamos al usuario creado.

![](https://lh4.googleusercontent.com/MrA5JPpqUrZIFgF4xqQdOHTbVLXcum3YjciVMUvPniUg2VVh06RNExufinZI2caznvpmBi8zJA-ggMGJ0rReoEgr9_P_zMw7UviA4e1tqCCIUmRGs_gBjc0DVMYm3F810MbnMsT7)

Instalación de SSH, ejecutamos el comando para la instalación de SSH server y cliente.

![](https://lh6.googleusercontent.com/ivtRQT73fdZOU1_V5HAm-1jMRuNTTQfkHW-xrftwzCERnhBEPw8Ma1BOCGwPm785QBTkFJNdOvxhs1_U_dmtd17yQKCn2rN-pBA5sLrQ-7e8Kq8z27YvCBfHS5uYa0IGdGjZaEiO)

Configuramos la IP estática.

![](https://lh3.googleusercontent.com/gkoLRaLm1s7Wj3FxnD_kdbboZZ_kt9PVjmFnqzuK0mfyZz3-SOmp7sAM3c9YwrpJoqZLLiL5Cwr0pauzGKLIM0uLpbGi-n7fzI5CxjzOVyPh8TB66iVKEyAsgbA9qHSiZrwo9qvB)

Instalamos fail2ban

![](https://lh5.googleusercontent.com/OrC4IeM53uZFc-vuJIxvv5UnoMOqVuSjssggajvvHziBfegAzQJYNWFhzsq28dmUk0THBeI8mCDR-FO1gwF86Z1rr0UwzVSkNIPPtFXIe56REYHzRnfq9MpdgYgl7Gu_ssfvitUk)

  

Iniciamos el servicio y revisamos el estatus de este.

![](https://lh4.googleusercontent.com/frl2zaAwmvcJIP3krypUMsj8bzUrZOLwI_qkvGWHzJKxoIkog3BsNX6t5Ke-KfqAYrTyUkGe9vUrDUpdiJc3hw7y_MFu3CgihAhSLlLz0krathK2h9BoHsVByaaNc4K82ijUSnMj)

Habilitamos el servicio para iniciarse sin intervención humana después de un reinicio del equipo.

![](https://lh6.googleusercontent.com/LdmZ_JCYNJMnZZd3Lmv2Vt_9kek2DFZgpqDpnHK_4rE-uiH2FD_ggzpUYgmvi6dwBYSCwVTrWoikC7XJSZGVMkO0OtYS-pQH6cvay7PtVsqXchIXR844BfbvT9tXwnRzNJRiM6mc)

Para la configuración de fail2ban el archivo jail.conf en la ruta de /etc/fail2ban, el cual contiene la configuración predeterminada. Para evitar problemas posteriores copiamos el contenido del archivo jail.conf a jail.local

![](https://lh3.googleusercontent.com/LoOgSi5k4npzFilcqZSct-GZhn9zyrENxfNwkKZZf8_KccSxRX3snawjU4Dp3CgthDWX88N5KqitH6IOfuaisnv6_CnXj8frBtDoriOk8DQ0AzVXj5rq6kfYBo14G_oeKx3UYaKk)

En este archivo podemos definir los servicios que serán protegidos por fail2ban, en este caso se define ssh.

![](https://lh6.googleusercontent.com/40AnvH8tysZ4utfoZAW1d84XyjmT9xTWyVR0AZcTyMX6NtJ3Gr0wPQ9NupiIZ8SfVzCJLkH1D3KxY9AbbTBIQuhWEHpTE9Lr7hR9o9TQaruEj1MuopzPOCl6CFc59AtORHTEvVlV)

Reiniciamos el servicio para que tome los cambios y revisamos el estatus de este.

![](https://lh5.googleusercontent.com/euENSPYkkBpjAEHcVdGvfqmu85UGg47u2gt2RcqHgfCM5yBaU7OQcQx8dEvVWS0bBD7GEWHydtoy7BDct6V0wlIvNuXLG_nepFxFxUEktDnfOLrNM31AhXMatMUjJvFZGXoeEX-Z)

Para comprobar que está funcionando, intentamos 4 veces ingresar con credenciales incorrectas al servidor, el cual bloquea la conexión al intento número 4.

![](https://lh6.googleusercontent.com/fwEZCNU-BjMk1V7swEcUxpy-BZ9P7a2r7EJIfKRmyn3ev3Z6QLjNpoEzm_kqPmHbwQkyhhN7cd5SkakX4af_89gTQA8dzA1Lx662C11lR2L1LMtcSJrvTukpykz2wO0uR6WpldrB)

Firewall en modo bridge.

NOTA: Esta máquina cuenta con dos tarjetas de red, en este caso una en NAT y otra en una red Custom,

Instalamos bridge-utils

![](https://lh6.googleusercontent.com/G4eEepDOfJL3CsEf8fv9JqY8h0wLL8Cl6hYpOnM7V0O7doMoycgCNs-MwVEFKlFBhl0howYuzViE72g3j04UBBbg8yWmoVps2hz4sDSGpkS5BlqsEMVYZwk3N3W6lItswALelt5M)

Creamos la interfaz del túnel y vemos las configuraciones para asegurarnos que se ha creado la interfaz.

![](https://lh6.googleusercontent.com/_lGKiG9KyBJGJbDlFijlQhlwPwQk7zrf-aXvGVmEW7ckzAVg9tbBCv729YWfhISjwYoSaYeoLZr41yA5GN0fzQ2xbn-l14MAXhf5X801OQw4J73V4JyYjxyhIo7ROIP4F2hScaDN)

Editamos el archivo de configuración, para que el puente reciba una IP estática, en este caso 10.10.10.10

![](https://lh6.googleusercontent.com/LRm0bVncUOu7MZsIwO9RBt0T402vcSqzv241LOhDIy7-NrPtt8FiFEqZDID7dZO1g8LFh_lnhcc-jzexuj5vMBT8OG3VZDUuDrQzZ5f1dAbvDrEWwhmKofbq6knJxwM95GXKY0qO)

Reiniciamos el servicio de red para probar los nuevos valores

![](https://lh3.googleusercontent.com/jlhRpGdzG9XdkMjQCf7LQlaKELlx58gmTdqIeJeX7QccqtgChnafbhJef8L07pNtG1n12KJfgjJTfcYU_gQJqhO6esZkSHDqmZehAyUqKpGYtwCS0B51a98NIqpmsOxyBeHLwkvL)

Para probar que está funcionando el puente conectamos un cliente, donde conectamos la interfaz de red del cliente en la Custom donde está una de las interfaces de red del puente. Lanzamos un ping a google y podemos observar que este funciona.

![](https://lh3.googleusercontent.com/Ip8vo7atYFZ7Vjs9pEoyx1gpps_NZTq9N7k896vq1o5JyY66VdVfSR3HVW0AmKbCVf9X3c2kUa8p7N-cHA-Vi7SvJP8wdOWmb_k6vGMZ96rRq0G2FyUCckmez8kBn20SwEv8yFUa)

Ahora apagamos el servicio de red del puente y volvemos a lanzar el ping, observamos que no está saliendo.

![](https://lh4.googleusercontent.com/xTX0G7tfAUok3-jP_XZgzfVmfkKWy_xXiZdkk4jy1Wb6VW-3gLhgkMrkuYFy_nlukrfVJUtbpegv6J4BA82qwlXmOKb0qD6SpHW0FDhVg6eQL1Oc5oeH-DPeveHN0hArgJ4hjty8)

Una vez se prende el servicio de red del puente, se alcanza google.

![](https://lh6.googleusercontent.com/dFF6tC2-q3Fie6e6jkUz8JKHh6aD-nXBBHR-HDnldr7Y_n8EZACtZckGaAcWulMdwO36AdjU7eOZdQb2NaVcnvk8trUE3QMZJ8Tf58iByZBod809kIcGK-ghoRqIpHKfcTeylDcB)

Netfilter

Para que todo el tráfico del puente pase por las reglas de iptables antes de pasar a su destino, agregamos las siguientes líneas al archivo /etc/sysctl.conf y se reinicia el equipo

![](https://lh4.googleusercontent.com/YGFwJHP-kX4_krSiRiJ3Ynr5uWvGPAZpYwBjcECByKZGO_XRVXiOLy2mm0-9B2CZxgCYVPnrk47S5d5QObAr8f1KOdjp0D3wuAeQ6Du-uPMB7Llyan_Y7LLlZKt1syC8N_EZxoef)

Habilitamos br_netfilter

![](https://lh6.googleusercontent.com/CSnJ3gOMqNC8D7Q4UQnZdMd6k-E1zIUo6yTqc4NnHWO8H3mDteH5zY5_NuWMoQFBX0r-ewTx9JlNjzHimYEvDddEk8bhaZmw2NzWCQiya8jgdKVSeGdPjN4olsnkNZaCWDDcNBXI)

Agregamos las reglas restrictivas para no permitir ninguna conexión, para partir de esta a crear reglas que permitan el tráfico http y https para el servidor web

![](https://lh6.googleusercontent.com/Fck_xHWvvaojladlStwHZpJdhtSyhOzoWK-OfASE_-S3kVqWyp7Zi1UNKRwoZBXalDFySEit4BxcXtSAUtwu0LkJ3xqEjj0Yeg1eK8TxDrVxdx-AXSDfObIGBADX5A3gTyUYQ8ii)

Probamos que esto funcione lanzando una petición a la página web, donde podemos observar que ya no permite el paso del tráfico por el puente.

![](https://lh6.googleusercontent.com/FFQo3aOwTrdRd3fDkt9kKMTGZj-tpTrU_By0o2RwVPIpxKdWwG_ziTW7z7ZpAPVfCDxl0pAq3JPXNzCU5EVvvnqlWPELsQ-ZJYw7jwU1DSAWuAVQ0LNR2Hxrh3G6bbYu8W8KeQuf)

Agregamos las reglas que permiten el tráfico http y https para apache y nginx, además de permitir el tráfico ssh

![](https://lh5.googleusercontent.com/MwTSPJqSlDsU6djxNx7XhNOnmiI8M4UMa7lRM77fKhzRW-1XhU_b0euzaM-lrjPgVXsVcMMG2uCUG2Wh1X3o6xlq1VE8SJeAL644D4MyxkiSrPRNCAQCfBbhm1G0_9BYcMxv-7Ja)

Para comprobar que el tráfico está siendo revisado, hacemos una petición a las páginas, donde la tarjeta de red de este cliente se encuentra conectado a la red de la tarjeta de red externa del puente.

![](https://lh4.googleusercontent.com/_fZHRHr7SVeUJJeKvEpzA4-zTS88UK8OCwu-T-Xl4gQDeRKDJs1Mfdr0hnsyLoD5rcV4--0HL3PxJ4rASlvBMEzvIQz-72UvsjtJ3msRhxN0BLfvmCql_X-Z2xMVZugEugh_mOkY)

  

![](https://lh5.googleusercontent.com/6O8oW8BcQv0Qp6f7UHS2uipfQVDfehm7IDbqGOb3YsuVEJpQUV9gogf964depZqmxkKQk-0JGXAd8Ft_Yi2ObIStyW109oqLNldC_beYak8wk_Vf7QsY-oImgONbq7w_bTVuP4Hy)

Para guardar las reglas instalamos iptables-persistent

![](https://lh3.googleusercontent.com/nwuvFMUBbklMl-uKh2fWpScAVxpbpgsSLWpXC7vWZ8LPil74f8RQf4Y5C1-xxe4gatjxOk-WauecKvjPIt4RTd4d9y3I7wgwgMsTR6ZHzvX3P7G7qoWmo6H0_m5PfXxd50Ypt9rP)

Revisamos las reglas y las guardamos.

![](https://lh5.googleusercontent.com/n4cgi1xhOk-4wyttsycO0elsWv_WG6tytunrDUl0FDsn7-uya_nYSf7nWJiO7W6KdHOixFBnjqDnWY8d-fJz26uq2vQOB3mRnR75xqTGCHvZyQQuMYVLyr5ee25etC0FaPrVkmix)

### WAF (modSecurity para Apache HTTPD y NginX).<a name="id3208"></a>

Creación del usuario para el servidor

![](https://lh4.googleusercontent.com/AaF8euWc0Hkc4ancNvpilnJ1PQZmrCMrJKLCzZIRx2R4hfps96DgYIsotzxkqLiGqgVlD_Xx_FfrpBjxHt-j3sPSLSqZDkf3aLzwfHuK60uQUaVpXncYaw0eMoCcw2AT4r2gm72Y)

  

Damos al usuario permisos para la instalación de los servicios

![](https://lh3.googleusercontent.com/L2QNS4_oggNpni8asuHBeGKUUVOA_hBMgHzDAq9kS3DM5yz25pevoq8lb588yl5N2WqvO9gYtNeo3qqT4bXG5PBGEjrvFJ19AOS-ZOTSTCTHRqx63nSL26HxEfuisU82p9TrB_6n)

Configuramos la ip estática, para esto se edita el archivo /etc/network/interfaces.

![](https://lh4.googleusercontent.com/cmf4r7KJ9UyrCCmNq2CJtAiHItGWlQqArkwl8-C0gg-p_R9FWWgSgyGXu2Dm0eVJftPuCeDU31ASQ_mF8knmN_eVcOrZHn060WA8fiLjRcImPqzFq-yexe4NdukhjxUqev-XCOkw)

Instalación de ssh

![](https://lh3.googleusercontent.com/_hNXQ8B7vGz6lRLewkU4UtLo8z3a2Qy8j9UGnMxnglSqcUM3riIuhnwJCm0Mrl5wDg42us7VtcUCPCUV7uxS5NeerTLWcNGcdBRKOr1KmLgiEdru6i0jMS0Ie67jg7yre9i_fqH_)

Revisamos que se esté escuchando por el puerto 22

![](https://lh4.googleusercontent.com/A7V-RtFbZpOyNLZvfBkzMgpfKzw-MJdSKl-INe2iVZdWWk3QbMPvEG9bdPTzwNfPFLYuAYTKlR22EpstcJd3ACuweJtUYJyuG8mQrlEDhf72IK-gHdwZS9NfhIGEteKRswKsuaUc)

Instalación de apache

![](https://lh5.googleusercontent.com/l3jPsroRv5cQ_8vaUEh0ZfCr_d0Xck71kiGBEomTvnVcZMstHKL-PI7cnnlQzgyoaFVqgzRjScBWEbJ6mljn18mFIUi8UFJ3frJj9dvD475kKCvCsFnZ-SAitxIGh4X0DRuzMFR0)

Iniciamos el servicio y lo habilitamos para que este inicie sin intervención humana después de un reinicio.

![](https://lh6.googleusercontent.com/T9USuIUNiaeU35Ii8HROwP9AvcnR_ZIcVPnIuCnOhfsalubHXY4BHc4Pmcx-e5yZvwQ1aioTnbuH8qOtX3Gv30hUnSApv11UnLYnnD6YCFSJfpyJn8TlsQ-0uZSADWsPpH0K7wb-)

  
  

Instalación de ModSecurity

![](https://lh6.googleusercontent.com/qT_8uwIC-f39Qp6MqqSvFE9esiHeKe1z8pinFsbEl_122ds9nyC4GbHlS6hvuhjEYXt14ky8oNuT4pUNtLs7wIq5yU6rGpuYgIoDKTRJK-xWUzcSUsh7eQE2A0dPAB8WWLsn7RyO)

Habilitamos el módulo unique_id y reiniciamos apache.

![](https://lh3.googleusercontent.com/VIqt2GMaGBYAiFInCmMzL7SI3xK6xb8KOtGAKRDUGU0cDeirVguCms7ijHMJ6ZnS8BAeR7u0B1kwQ8IWqE4GYMDYXXpdo1HGQaaWbg3HSJ3lONd8FhOpNrPJrPHAz0ACYIdeB4ji)

ModSecurity trabaja utilizando reglas para detección y filtrado de ataques, por lo que por defecto incluye un conjunto de reglas genéricas mantenidas por OWASP, pero de igual manera existen reglas comerciales. Por esto ahora configuramos el set de reglas de OWASP.

Inicialmente ingresamos a la carpeta de modsecurity y movemos la configuración recomendada al archivo de configuración.

![](https://lh3.googleusercontent.com/PEqFgeoefV0Yhihj9guVTFtXTKtlfpDNvNZ9fXWB8PCfLDSFdIsvkTKRRCwVW1DWXbn1NWkXcgj0XxPaJvHFSYikJTIKfQJVpGyKIZTmZFKSu450oK-qBu_58JWWjMkeOxa15IF9)

Descargamos el set de reglas con git, por eso se instala git seguido de la clonación del git con el set de reglas.

![](https://lh5.googleusercontent.com/SC2sOKZ_vEI-9ouyEo4tx_4fD9lnJqsrcHZ4wOW1IPvmyj6-Uo5lbErEsw_ulexFs0l0TtR_6repRZmkSojm8TPe5oCGuYW-Dsy2tu0169lsMwEBoTAyfgG-4SNXBKvePVJrmHJl)

  

![](https://lh4.googleusercontent.com/femf5kVzAUUOSyK82i5i4dY2lB2eAPxqJarkFuPFqNBVIpAK9nu0-6kXUN3691uQMw6GSWmzwz-1AnQ__fNo2V0zyC0zv2ic7fTrBq-MSXOR41mq-6Zv9c49-rHAe90vDL6JwfLE)

Ingresamos a la carpeta y movemos los archivos crs-setup.conf.example y aquellos contenidos en rules.

![](https://lh3.googleusercontent.com/PjTTzDOK8AI3A9a8WcsZdoZtF9HtZPhaX2IxjqND3RAxjofsf_OqYzkIs_cwnhn_wVxMnUQzd18zUtkF5xK7KuaOEa79QZJvTHsX8JBnO0TQ4NJTEjAjRppPyEVcWVcHkaOfvlfu)

Para aplicar el set de reglas editamos el archivo /etc/apache2/mods-available/security2.conf

![](https://lh6.googleusercontent.com/KljiKBVBt-W85Haa1OlP-mGSvEeScphHq3LwwzEiHt29YfOrEykz6ITZog6rbZIxYBb4vpJVhXC0ZRAeXjS8BLtK5UreKyXklk4IOsK3J5RmbmNdJbp0mWxalQxCnRsCW6obItab)

Probamos que modSecurity está trabajando, para esto editamos el archivo de configuración de la página web.

![](https://lh4.googleusercontent.com/VtUW1ZIocg06FzVJf6qcYEcuAlPk-9QYJcssbJFaYcUcyMC1To6xyuWLJNZjmBF-IH0vGKt7xFaVO4cAPh-Kxzil8hOYuBFUlJQCN-aI8somJn3lRKhvX5T6ucBnhx0cEmWKqQxQ)

Habilitamos el sitio web.

![](https://lh3.googleusercontent.com/L0c2Ef6RNDI9PrR-SPeS-60fU1i3Xo2_SaNofGh8LzMTKB_UiLNUE92I6fUFrBRvPOKHjY-Gvr_PQa1wCRBWSq3ENJA_ftZT6Z0AyBP0AayTLbDBbDWotV4tqGMZ8pnbw2icWC13)

  
  

Reiniciamos el servicio

![](https://lh4.googleusercontent.com/j2CcvUa8RhZMM_AIM55MgqeW5Hys-IYlRF26zEVc7474Va6E_lnEALuWIBqApbiQDYX517r4h_ZsiFl-_ibJvbvlzLsQM5rIil-kME1OFlaQUCcHZUHj7ZveJ0Ew2unT2OypHG3n)

En otra máquina que alcance el WAF confirmamos que se tiene instalado curl.

![](https://lh5.googleusercontent.com/20by-bYVbJfuhTQTv04p2rgutYxMMrhYX-mpfWZ08aEltSQZRN73482cprvFw4GHdhdmFRvtCndO6HLg53X6spP0IdlVAhMk5GULNMYIKgfa0VWO1ZQLkSItWLex-wPj0A20iR0F)

Lanzamos una petición

![](https://lh4.googleusercontent.com/tkTwYuO0ABZdvW0GYigLboTmrYlXLU_FT6v84aavXKretufMq_HQOQmx3EbdoJNKqUKu3iu25Ldij8bOSq814Je5uHaudKTXxOBFu4Ngag3WhgNaBV2Jz1nQMfGW0C9D1XE5n0E5)

Revisamos los logs de apache, donde se puede observar que modSecurity ha detectado un ataque.

![](https://lh6.googleusercontent.com/_PMiCso_8OLimKiAVJPZ6JfIz1EjT_BlJKIkR91UUOW97fWzylxofgsqh70ZiY6WkbI2bbiBLrFdr--MQ0r5v2P96IUvd16ejdyKgQX96qIx8cBpFHLQhEGi5CWFqyziBsiFqG4I)

Proxy Inverso

Instalamos la librería.

![](https://lh4.googleusercontent.com/6KrJopjjSRrzLD7N5xQWqDUwCaLbuUdErQaBwbnMMJ46eroFE_Vp8_252Oh9nlay0qGxf5WiERkrVDSiz_VKx4IIYPe6uTabPX6OeDmUH7bEi2tkIPYyyQ35xrfFJ3C-eYJC5vB8)

Configuramos apache para conexiones al proxy, listamos los módulos disponibles.

![](https://lh3.googleusercontent.com/mYXRYfSuxA2B1AP3NLz-ufw9I6lN-Dl93Etkxa7cB0MhGAl0NlJJm8bzXZ3gv5o5K0y_lmFaHCgPlmBoVHPDdWX_G1l2oJ6IrqRozoO_1QkGOhU0EEdRzM8b0w9G202WLIpD-gnn)

Posteriormente escribimos los módulos que queremos habilitar.

![](https://lh5.googleusercontent.com/qDJy9VHvWuWkswzVQXruUsJZCR1UxW_IJPeTqCbvw3bSuQrA-03PlGE2PLB3fm-4D99BGWDOOtao5Mqpeb_u4IdPhm-RUH50bAuSgSSHzFYK3_e2Yi29V8NJkKsFlcVFXQJSdHmU)

Por cada página, ya sea por IP o dominio (dependiendo de la configuración) creamos un virtual host en nuestro servidor proxy, que redirige al servidor donde se encuentra la aplicación web.

Apache HTTP

![](https://lh5.googleusercontent.com/-X2XYOffgoT35KQFrpZOXbGesCY7JApuA196GH42j6zeewZIWh2EaStqfAx5Xsc42ivRuSAUDFwQ1fPGqq7zmi_3aWFgf6hkMFPuaBgYOhsVElwMHjoLTk95j-2hrd6RoLXbgVgW)

Apache HTTPS

![](https://lh5.googleusercontent.com/HdmeNMyU4miEi23J32gyK75IiUSADIekvTF0QTuDDVpmx6QY8c0FX9tcrRPZQ8rltrM_vQR_NWCiqFr0sZGwUlkAD13iqXiH7hdok5ML_FvdTPjN2Bv_BGCTUp2cYXOuU_yr0mBV)

Nginx HTTP

![](https://lh6.googleusercontent.com/USI2Ndv0HkCpYwkk9Vfz0pJPBQqhJldNLoV0NRdPFws6n3ux6390tkDuMj9Z6vo-5pwWcDcRIObiklyGCymoQckBGPJ74EKu-fqNR_9aJoc0t1IW6XUX31C8ZVkLYslQOEE8SxCN)

Nginx HTTPS

![](https://lh3.googleusercontent.com/hGW0CMe9hJiOYN7sKMZIUpQ7kf7TdamNRoWu8tI_HwVI45oT4gfAT14-WsuNa47lgaIAv0R-fu27Z27ToFhCPpV3dQukc1NfgfYyuBQ6BIbTQg0BYjUor-_deMSbdSgrQA4SY5YC)

Habilitamos el módulo ssl

![](https://lh6.googleusercontent.com/W6ug4y2yqaPhm0ioHCkF5MDIzj5iJKTcdPWQ_D5W0WL6OxBZZ7JKkToIQ7JY3vUFX3l7GE4WWXE1rMmx4AJAF-mKTIMYLaCR8Xrcmqy5ZfPUpeqUZqs6tFOeBTXC4sINOh3RuPyx)

Habilitamos las páginas

![](https://lh5.googleusercontent.com/i_90MbrriLp4iSBoG7Dag6-2D3nty59esty6ZdAoALJRvQlBpM947UyLwCkAbKL8Z0Cu6jMrVuE03LmT9nQbfU3oGmIKnJyCl2lV270fr4Slo2INBiPgmV-BAHtQPqtPn7SwjqFe)

Una vez hemos pasado los certificados a la máquina, descomprimimos el archivo

![](https://lh6.googleusercontent.com/XZmtAuaJlCKdQKi58h3oz3qOHFToQ9_VeV40J78EhozaM6SPF7RIrV-b0trrmPwjJUHWu9ungs-86rw1hzewJb-RnFOqrHzdiRQ51GQJ8jeeOC3PrqCHCBog_7dA9TibieTuoTlu)

Copiamos el archivo a las rutas /etc/ssl/*

![](https://lh3.googleusercontent.com/IpfSrKkphn-2LTCo9_SXohx-BCs5p0Tpk9WVfwK7jU1YdWAPL8Kbh9tmiTEH307iTaOwHqJ94xhOHzXqsfvmg74gkSZYIPUBOV-qUS964iPddb_mE0WBz25ViG6_l3a2jy82l9Ie)

  
  
  

Agregamos todos los puertos por los que estará escuchando apache en el archivo de /etc/apache2/ports.conf

![](https://lh4.googleusercontent.com/7h1fq0zIkpL4QRiFJZj2bOR5KtnlZrtj0FZSM4gTXXWiwPnA6EerO4LWhgQCrjEgadZHDJEPH4sPoDga0ANGOopCkzM1gZdKnoMNR3WeyEjbQfbQBOWdF_SPUvt5q2EpsoAwNt1s)

Con el navegador visitamos las páginas

![](https://lh6.googleusercontent.com/Y20T_54YFWFsFc65_5iGgc9cS1QBENwfxpFTRyIKKU7ZAHKei04TIS6mgAAY5PZMrBp30SS_jPZJVqYc8GFSYp-Gvvxuo9VNA7hHolttdyNQx_jBF0VNCSoDIaWDvyOZp9wiTarb)

  

![](https://lh3.googleusercontent.com/kfJc_tiPufx9eFXhHeWaNuJNb2kztMTIsf48DxOp9lKqeB40b0ergRvw5IQPDPtV6s5Da4o_O7sXmTvcoBl4-O2f80J7i8hlTSL1CkxJ72swC-niQptv2LonNJxYmnXLC-oB4hZM)

![](https://lh5.googleusercontent.com/5Oe8tj9pby_PAvqGEgBQAii9XSVg_gF9yAqhXemK7ematSFwM6ym7Ap4Vx39kM42PZhLkMIIESYVH_Lw43XB71i8shUiMsEJZiXC0QkqRfhH7gb4swS4bWoMCqb7G6D_9HiEYkLq)

![](https://lh3.googleusercontent.com/gKzdDqbYLomd91NNc6Vig437bP40tkhSm51GaPlWcrDNPWnh4UZ_Gfyb4E4OLobBR-XlW2dHydEyHdlAimY4XoZiFStYDfrV0Ufff8XsFL5KpidaXE1yn-b_v2bvJIbKxZSg10W2)

Para comprobar que efectivamente el WAF está funcionando como proxy inverso, paramos el servicio de apache y mandamos una petición a cualquiera de las páginas, por la cual no contestarán.

![](https://lh4.googleusercontent.com/aLk4R67rRu9EFB8ZKtDmjmZRmEu8tkNcwufJze3N6edNl-wRiEubJdfD7QJlit8m8Qm7FbwNQC8YAmRAvvnMvOky-GtvImZ5qo2RsQL_T5OEF48aeDPqSBbUHtH-oVpJdRPXK3kn)

Al encender el servicio, la página responde de nuevo.

![](https://lh4.googleusercontent.com/NQhZjuHp3TWib_d1gPjd93yiXZApryfp_yCdwxDuwLSBrG7k5n4dxzKFxT9O9sYWxJnd94AtiLaJGX3d2hYBir9YBgRBQdLBobq6UaMdESXyBPFeZj8iuZe2eMO7IjoNSUCZhen5)

Para probar que las páginas son protegidas por modSecurity generamos una petición maliciosa con curl y revisamos los logs de la página atacada.

![](https://lh4.googleusercontent.com/R60eqgj9FVVa-NpJkWel4CK5YpdchmkCZ1H6DXBBRVTZbc5mjxtm-y-KPGznB4j-hWmZO_X_rdCy0h95e4XXwENKMcVc0u_u6SrgSmku7YU_p-ltdsvwmQJ2Iaod5_8ouMHzgmHa)

### PostgreSQL.<a name="id3209"></a>

Comenzamos por instalar el paquete. Para instalar PostgresSQL 12 y todas sus dependencias usamos

`dnf install postgresql-server`

Con esto el software queda instalado y ahora solo debemos hacer algunas configuraciones.

El paquete Postgres que instalamos en el paso anterior cuenta con una secuencia de comandos llamada postgresql-setup que ayuda con la administración de clúster de base de datos de nivel bajo. Para crear un clúster de la base de datos ejecutamos

`postgresql-setup --initdb`

  

También habilitamos el servicio para iniciar junto con la máquina y lo iniciamos

```
systemctl enable postgresql
systemctl start postgresql
```

Ahora que PostgreSQL está activo y en ejecución procederemos a crear una base de datos de prueba y un usuario que se podrá conectar de manera remota.

Entramos a la base de datos como el usuario psql y creamos nuestra base de datos de prueba con

create database database_name;

![](https://lh3.googleusercontent.com/2f4QOHGKuV4lw0eooXbZl7kJ7GQ8j3hvn5_lJdDBu2rMQupioNpL2IpsQyVuL4E4jzboJVd-_Mun0mM-T78juvsjDAFG88K0nWMH7sjN2xCZxMlIjsYdk_g8uE1BIQYbsH2INAZ4)

Ahora debemos crear un usuario y darle privilegios sobre esta base con
```
create user user_name with encrypted password ‘password’;
grant all privileges on database database_name to user_name;
```

![](https://lh4.googleusercontent.com/11BVDN90gqqZHlg3otS4zQuWT1Knwluuc0b4yzldaEeBjrx5p7ylX9suSy8gasbGQsIXxc4XwpmCcpj9vmvokVzR-bAu3brWghWjCrjak-m1ysUlMbdFzz-l7ijpnvMoBwHA_X_d)

Salimos de la base y ahora vamos al archivo de configuración /var/lib/pgsql/data/postgresql.conf para permitir que el servicio esté a la escucha de conexiones. Cambiamos “listen_addresses” a * recibir conexiones de cualquier ip e indicamos el puerto sobre el que escuchará

  

![](https://lh4.googleusercontent.com/JOpdZ-AbWOc9y1QpoDhQJSHzSddDoILFctbE7oGynmHM1URl6wtew04_Yi7solcOS_c9mPIbyszsiDEoTs8IfwDpL7v8R7qsyb-QnHpGABXxyQkBumcycUTWhA_6c09Sc58L3GCL)

Ahora se edita el archivo /var/lib/pgsql/data/pg_hba.conf para indicar el tipo de autenticación que se usará, si es local o remoto, dirección ip o segmento y la base de datos a la que permitiremos la conexión. Deberá verse como la última línea de este archivo

![](https://lh5.googleusercontent.com/PzB3XbZJSCTL4UXwWWWALn3WSDeIAz0tX0Pc2fuhAcG_btOgDNMhedKBy1FVwMDo81FxPCYmSH_EjjbtOGBunwptC3Um1ZeJPTdT1Jqq8mf95DTd3XpjNLfJf4mtWxnnBPJsFofO)

Por último, debemos permitir el tráfico del puerto de postgres en el firewall de la misma máquina

![](https://lh3.googleusercontent.com/v0YK_5kwrMLpLSVhBNca7C4nV9Atfcg-VFYH7g59UZD1EDz5qVSepqXMX53j28hRHtGr5ckPRcj6p1MiavuT73snbotTJ636EO5uiC9qXSaCGKubok9nJHbrNq5DSLdezv4LW9RF)

Probando la conexión desde otra máquina con el comando

`psql -h ip_del_servidor -d base_de_datos -U usuario`

![](https://lh4.googleusercontent.com/GcTJtnZGLPp61iP5GSii8ciFVU_q2xp79Mknub3VvYBFoZXlraWztJH00_oAxPFe4GPidutoZlwgzqyRyq8ZzzsiPou7qSvujhIuT3VKM9cP_Ms_WihzsnNPrcFD6REA5XSI3Uwx)

Notaremos que el servicio es funcional y acepta conexiones.

### MySQL/MariaDB.<a name="id3210"></a>

Comenzamos por instalar el paquete. Para instalar Mariadb y todas sus dependencias usamos

`dnf install mariadb-server`

También habilitamos el servicio para iniciar junto con la máquina y lo iniciamos

```
systemctl enable mariadb
systemctl start mariadb
```

MariaDB incluye una secuencia de comandos de seguridad para cambiar algunas de las opciones predeterminadas menos seguras para aspectos como inicios de sesión de root remotos y usuarios de muestra. Utilizaremos el siguiente comando para cambiar estas opciones

`mysql_secure_installation`

Se solicita la contraseña root, que no se ha configurado. Pulsamos enter como de indica y luego establecemos la contraseña. Luego se nos preguntará si deseamos remover el usuario anónimo, le pondrémos que si.

![](https://lh6.googleusercontent.com/drckW7_aQn7eh8-i8qZPATdIXGa_QxlRIxC_JYX9dtAzl4iGZEHwinAp3cb5u9sOY5QA5LTj11WEuNtmHqV9yu8Y3yxPAswg1X-tP3JLRkDtCLI_XxToraN2WehD4ahpeKoTwSwM)

La preguntas que siguen durante este proceso son si deseamos remover el acceso root de manera remota, remover la base de datos de prueba, y recargar la tabla de privilegios para que los cambios surtan efecto.

![](https://lh6.googleusercontent.com/XYausWHtQh429ewFxeXRvTFLq8DoMLmKwn3JR1MwLjTPW0RfLtXFFcIL8d4ASsvowe2nF-MBv3hZy9kYj2fMeQsJmhBFwU5Ti0iHRC7DX3aM61XGrF_LNI74uILtzjbwh5H9_aAz)

Ahora que tenemos el servicio funcional, crearemos un usuario y una base de datos para hacer pruebas de conexión remotas.

Ingresamos como administrador a la base de datos y usamos los comandos siguientes para crear una base de datos y un usuario

```
create database database_name;
create user username@host identified by ‘password’;
```

Ahora le daremos privilegios al usuario recién creado sobre la base de datos que también se acaba de crear

`grant privilege on database_name.table_name to user_name@host;`

![](https://lh5.googleusercontent.com/yDBUpjS7RdF1n2jV96W0vvtSio99GJbpjBmk848g43vXIxyI-Kkd94ZHFsVfd9IzNn8qtbgacb9j_L8EEf0uOMTkvXEZn2PDGg1xmkW-3CJQHpgAyxbpiNHs8QbNesZqcIvYExvE)

Por último, debemos permitir el tráfico del puerto de postgres en el firewall de la misma máquina

![](https://lh4.googleusercontent.com/NrE-n6ECyzqFUErBwjTSkhP2n8OiIJElApoxAxFOsWfXMivd1tRTTibFU416Q3CPA-l8m3jU6AIf5IEXRqj_jDzn1H1ifYaArTYjJNTWR4-FE3zXvNF0S2uCk6g6Xd6AY69SZq8B)

Probando la conexión desde otra máquina con el comando

`mysql -h ip_del_servidor -D base_de_datos -u usuario -p`

![](https://lh5.googleusercontent.com/I1vlW6aXgIWACkTjevCrSShrdfFatbSzXfESOQIifP9hrWQQ0SVRMlQDfFYFE8lFEW6EAl3jeEp1-LpNjuhY_6asrdnxsSafwBRJatWnr3fV_lQwhWliCSdWNW1Dd59D5QZOTjxu)

Notaremos que el servicio es funcional y acepta conexiones.

### ProFTPD.<a name="id3211"></a>

Comenzamos por instalar el paquete necesario para tener el servidor ftp funcional con

`dnf install proftpd`

Ahora modificamos las reglas de firewall de la misma máquina para que permita el servicio

```
firewall-cmd --add-service=ftp --permanent
firewall-cmd --reload
```

![](https://lh6.googleusercontent.com/-v8Qgl3RGIwlDjUlCKVsZshOPnt43QigUMip5-MLtiFR8SZPYm08Hc6GI5O5N8J34l2MQloY0sbpyPAkHnsuyFMBUANHSv_m9haHVPwMDjgMV-8NJecQO7NO3ZayRzy79zv4FVMs)

Notemos que el servicio está listo

![](https://lh5.googleusercontent.com/3R2eVjC5NqyxWZPpFxYcfQZxMtLMQ5jg3UlYaSbrExmAa2LnTQOzpxi9zytMl0GD1A-TR3uEhOzNjfCex69C_uXAeol0JL711Lu98N3RKzKvcR6OXYi4h46ZkyTq-0CI-BEPeAcm)

Ahora probaremos el acceso al servicio desde otra máquina. Debemos descargar un cliente ftp y después, mediante el siguiente comando, ingresar.

`ftp host`

![](https://lh6.googleusercontent.com/dA-XGHj-4hH5qahIhMMMdNq66gR91cBDlZRXTqIIO6pLiXv2fS6S5890TZckerjJzrRxYf62yyKFF3rn26924Zzt8osWE8IgRm0e0T52srBlY0BM2AP496qWwPuYtf6dzSr-bcK2)

## Dispositivos de red.<a name="id33"></a>

Para los dispositivos de red se optó por utilizar GNS3 debido a que este servidor nos permite emular distintos tipos de dispositivos de red así como utilizar máquinas virtuales de VMWare.

En la siguiente imagen podemos observar la topología resultante.

![](https://lh5.googleusercontent.com/FjGLMYGbF_wVI9EsVV2f1Hooie4qukehWCpO-GnhLA82Abj7Jivstb2deXWTPVCAzDisJ6A5Z-oyRTxFzkXDpdTXVB4fTa_CMBjnTAuz13otZnJJqNCRT5gcay1r2uJ_xpm5EowS)

Se realizó una importación de las plantillas de los dispositivos de switch, router y fortigate que son ofrecidos en su página.

![](https://lh3.googleusercontent.com/rPGdlWR6zRA-P01Do30x8fGsFr7-7AhVdfHMULo6_ztY6uPkAYUcdIp2sEXUiZQWTIKUFIqSoxPni_l2gLLqMi6-bND48KxU8cODEcqotrMoIvMfRJnRDvn8jFhihz6ujTXsKGPI)

Una vez instalados las plantillas es posible crear estas instancias y configurarlas. En nuestro caso se configuro el router y Fortigate

En el caso del router se asignó una ip para realizar conexiones con el fortigate. Se utilizaron los siguientes comandos:

![](https://lh3.googleusercontent.com/G1sLeQhWzX996Fs3RlpQ0LWugKMdQygltSrBgStUYykDutcncyZDSFsm28ZQCt7P8tHLhuanddUj2iBczH7G4oXr8Dkmq9tilh7stBpflp9bVqN-4fjLI7-ddmFUGGxLvy55U37w)

Posteriormente pasamos al fortigate para configurar un puerto por el cual será administrado.

![](https://lh6.googleusercontent.com/q1wUH_MihXW3pjrAMNIXq6zGvM-c36_YbIdx6jJ9QvDA7hbYgui50dTw0rcWlgf5Oa0aYeGBWaCeNu5RXbSEm1AM9wdHTp1Ybv4Be3zrdkUswdosy7QiUnTWHqsXiw4JN71YER09)

Con esta configuración es posible realizar cambios mediante una conexión por https y utilizar la GUI de Fortigate para simplificar su configuración.

![](https://lh3.googleusercontent.com/2OXHh0kME1pVFPNCHzQXFmmqxM4aQMVUrd7epCULJMMAt0MBveEV2ZziVPqiTo-HJ6617fwb-_BRcJRtvi3aFVcWX25TqbQXLrBB77g4yS0HBYhOChqGpLRK6RukBy5xDtOkXFOk)

Lo siguiente que se realizó fue la configuración del resto de de los puertos para permitir la conexión de los servidores con el router.

![](https://lh6.googleusercontent.com/kjYGcmNMvzBmSIRcqEX7vj63a1SsMgFdh_jJVf2GoQhW4-weTOsjMYpZiVHjea3GCe3DCHiE1QWNV1aTDOqeyUiSIAYXaDLiw1329YTVreDfqTc9_omXCtGdsTyBYhzsvDmp8c1I)

Posteriormente se cambió el modo del NGFW para que esté basado en políticas.

![](https://lh4.googleusercontent.com/8bJE_rAWIFlXimr3w-E2Lfn7HMqt5xg3wFc7LzA0puYrBunHOlZWj00ZlT0YJwULGMSSPEm4Zw1yW1a1M88LKxnTG4EAubo90zNE4tu99IzD7baPb0k6WclFk9DaG4v6hNJmhOqc)

Después creamos una política en la cual permite el acceso total de los servidores hacia el router.

![](https://lh6.googleusercontent.com/Hjx8zt6kiVg9CvVpqpZOCQB-WRn5NdrmT01uosRcP41jnyThMkru-fKjDgTE_5ZNj2JskxZkjnzkg9BkQARVsQm3F5R4uZp7GK6_ithf1uuplwaUUwnw2J5T97S46dG9kw1SwyLh)

Se implementa una regla de SNAT para que se haga la traducción y pueda contactar con el router.

![](https://lh4.googleusercontent.com/ctlY14TNLp2izOrmrORqS6hJJQ3kovJQCVKL60-PU7_03UWWQBm2Smc0LkiFcQW9vTsD44Q060Sx6inYRkyXXkxIYzlc1a8Aa7aHjAcFmOfHfEhe-KbyiQOMhWTxXeymOrxJob3a)

  

# Instalación de BELK Stack<a name="id4"></a>

En esta sección se describe el proceso de instalación y configuración de cada uno de los componentes de la pila BELK.

## Beats<a name="id401"></a>

### CentOS<a name="id4011"></a>

En CentOS debemos descargar el siguiente paquete.

![](https://lh3.googleusercontent.com/Ti9Z_jzV9qlclQWBEyFNYNXXf6aGuYoBRbONhlGPbHpNWXEnGIl8AMG0O5IkUH9o4pr4aL_FYMcOOXoJ65CpMV49MD0i5lh_Hz6c37z7N1cEQaK21ZLSYiuwL6ywjANQTckDQ-gh)

Posteriormente podemos instalarlo con el siguiente comando.

![](https://lh3.googleusercontent.com/0jsyqBskq-5WESfKWesA3CyveE_GvqHnzAYbMI-pb3lnYQCLk47dKGWAw0dST4xtSbyQxol2irE--EBV4auE0QpSt6jeoUzGKiKcTG69wbvEvKyy1NNBt4Hp9BNKRrdtPLO3xnUJ)

Ahora debemos de cambiar la configuración ubicada en /etc/filebeat/filebeat.yml. Cambiamos a True el valor de enable para que mande los logs del sistema.

![](https://lh4.googleusercontent.com/7ukI7cQkKF1H4PlQHtoboOxFCEXJsL_0tWsIfT0BGUhuvjMUBXDcJD7LM1uel0X_T49XRUOC1kRNmO97xJqNtEpbWOt_rIrFe9ret_-MEL0WXorRL8i9XOKVzlJDOqFwupwzSGTP)

Después comentamos la salida para que no se dirija a Elasticsearch comentando las líneas de “output.elasticsearch” y la línea de hosts.

![](https://lh5.googleusercontent.com/bGUkkaMrjn3CsrMWZR0Gel18WuyEjq-TK8U9IBAYY5xfMs5weSOryP-x8WUefp5NyreR_kVskE8BfTyRDKcVjZvtFGvFnDXuhdfXJEUb1A6mrh1kV3UhXTFK3zg2qtukjbwQChSJ)

Y descomentamos la línea de output.logstash y en el de hosts colocamos la ip de nuestro servidor de logstash.

![](https://lh6.googleusercontent.com/9kabNukTqPqeDCS0Zhv5iHtHzqrshrhqkCo3DGC47bZ8J-P6T62FWGOkT5oi3D6wFUGEcvFqNMQvdYCj_QoTKF8-kyW3VVR7Jd6Dswrx43Al9BKGY2T570WrioHdD3SXmQ-5ajcL)

Después de eso podemos iniciar el servicio con el comando.

`systemctl start filebeat`

Y para habilitarlo ejecutamos el siguiente comando.

`systemctl enable filebeat`

### Debian<a name="id4012"></a>

En Debian debemos de descargar el siguiente paquete.

![](https://lh5.googleusercontent.com/SMFBAmzqK62tOGL99HQ5HMeAhwzvSwxilb1NkgB0vfB9aD9l6bU_mVxEbVqnuWWfZ7FH0gHPtXuO0aLUC1UXn1U7W6fe-hgX_FSPp_UEf6XM_7M-m3L9n6WZmys4Efr9xw8DbjVT)

Una vez descargado debemos de instalarlo con el siguiente comando.

![](https://lh3.googleusercontent.com/K8M-YqCagvPyHMEc7YTn4193vDrixkUoFast504ksqb3b-5QzOG2-Hil5ZlLiDbAxA5-ygYdvec1oIlGtk4bmaCFHN1GwCUvDgcq2ogwYOWB87R33uUIJ-x7OWsxAfIjo5-xXosG)

La configuración de Debian es exactamente la misma que la de CentOS por lo cual no lo colocamos debido a que sería redundante.

### Windows.<a name="id4013"></a>

Es necesario descargar el siguiente zip desde la página oficial de elastic.

![](https://lh5.googleusercontent.com/0nZswtHHppPHhCKwdKJ_LYjD9-srlE6FdAfuxGey8H1t6RiZ5VM2vE05O_rCesMiR0kG8bG1ThQtBEh0rVckG6H6gqDw22VHbdjUiw-dX1CXL17D6Ox_u7oPCDAM5EQKThDFNmBb)

Se descomprime en la ruta C:\Program Files

![](https://lh3.googleusercontent.com/Cpu0rIF8-qtpvSlGElgAM-a1JpA91EarAxwAKbTAU_lKe5y947ZJb6Yna64ilNqIAwoOD8ULeC0SRx_xgA1v8uZ3T2G5XnWbK4sH3chfMlyaCWD8PkU69ESe6_2A9SV_CqmPLmSW)

Después cambiamos el nombre de la carpeta.

![](https://lh6.googleusercontent.com/RVsNAV44WVFhMdZqVL40bCryu4cE2H4U8xoj8LxhNdfULeUthQK-NB3qEgKadf-wTyjcpHXo5alKeo5Bl6-i-DCC-lHYvkJA0IGKSxJ5kYrt2AKHjWC7W0tiOgqm7zx3Cj07u-7F)

Después lo instalamos mediante powershell.

![](https://lh4.googleusercontent.com/aUi4YLmyFVYa3MGekDhrcidLwBMQ2QOq6e-oAHOzxtf9Rn1NKar2OLni-MhnURi_y0CF6xfIRpPlHKjW5X08_PO40djqsIGzDsougv9gwKLVxMYFNt1q81ctY5DNhttbTu6h-qXG)

Después realizamos la configuración en el archivo winlogbeat.yml de manera similar que en windows.

![](https://lh3.googleusercontent.com/kZq-VdD9sSAlG-ovJsjYn9FCf67ko5WdZoUJdscIKoeabmbjzaJECmNKmFbQlnrllZ_CPaGxD4YdJ5A2s1lIkkKALM7x-tDNurOjDN4aE-GuuiKmUydJpORovrI7d7YvBX3RkBOW)

Después en services activamos el servicio.

![](https://lh6.googleusercontent.com/onUTFgT2EDQ5PiJkrYiCxWOLvj8nno7ZNgWShp3Ll1Uz8sCaYjk4CpUerQanPSy6_3eiZc1H14HhiBhsg0qmHw5JqKHD3-c9i6C89gQ1bhF4Uq4Fp__qKKhG5vSGQVnbUAh1yNF8)

### Dispositivos de red.<a name="id4014"></a>

Como en los dispositivos no es posible instalar un beat es necesario mandar los logs a un equipo que sea capaz de procesarlos mediante syslog. En el caso de el router y el switch es necesario. Escribir los siguientes comandos.

![](https://lh3.googleusercontent.com/G2QaHV0tCtJWCyUHL8LgOjM3TGs95vmvtbh6XCqxVeotCBEyq1HCoxIdi5Hf1m5ImU0xhNgnRJVwEime-HhSI0sEBE-B8K-J5SjSFTuds7Zfwh6mRSOmopPHt4Te6wH-qWpXgzfG)

Podemos comprobar el funcionamiento con el comando sh log.

![](https://lh4.googleusercontent.com/I442FlQ22OLMhHfwp8u7VdhYiy9Q6pgHiovKf_ePjEKA1TVVAm07NHyCh7IAkTmKYzC5dij5p98gvWUQPKYwQrbSL9W6mi1wSch56DWDN_gz-ytg2Ip0hKTGrU1uKS6Zht1e3js1)

En el caso de fortigate también activamos syslog mediante los siguientes comandos.

![](https://lh5.googleusercontent.com/hXI_afW3GEkZJjD3RGx1LTOUNwf8sLx9khe4Etqs12EGiGjCrInBf9h8uOkVKbeiBK0AzDvcrHrAvkVgmNzt9-VHp-Z-dIVdGumgFNCGQrrS70Ma1TpiYmo5_uEvmV-3YMrGZ70X)

![](https://lh4.googleusercontent.com/yCmhCHPGcNr7BRIJZb6Tcmo3HgQ2Bw7akCZ-iRdTEHXAaz4c38NcEIbiuAkt7psspAs04oP_LfUK57UPv5VhgJPlm9EUCNTfQYhjVIuBMreYVAbs1Tb3Z27XoEJIKoCMVMpBkf1A)

Una vez terminada la configuración en el servidor que recibirá los logs es necesario instalar filebeat y activar el módulo de cisco. Para activar el módulo de cisco es necesario teclear el siguiente comando.

`sudo filebeat modules enable fortinet`

Una vez activado el módulo debemos de configurarlo mediante el archivo /etc/filebeat/modules.d/cisco.yml

En el caso de fortigate se hace uso de un filtro de logstash para recibir los logs y procesarlos.

## Elasticsearch<a name="id402"></a>

Lo primero que debemos hacer es instalar el paquete gnupg, el cual es una herramienta de cifrado y administración de firmas, este paquete nos será de utilidad para poder agregar la llave GPG de Elasticsearch.

![](https://lh6.googleusercontent.com/5Gt3SBVKmTVVPNSZukEoJakJyOv-4DY7-Kl6DVeu_kK-8f502cYpk6rvhzCOIduLOIZJVcgb02GrlFw-SRA0MgYyAPMcNyVNvqmCLrFd_6PXz0VVoRNVSW5O4zhDDgVswPO4YLoW)

Una vez instalado, descargamos y agregamos la llave de elasticsearch,

![](https://lh5.googleusercontent.com/e1fdwoGAU3lkdTUedS8inStACacoAtgMowNLSPn39nFGKgiFIv7RfY98eWrrdBZhefCJDiF0H_afq_bFcQcM3SWcpm5ek2eYyuxmbUYjUCrZD9gi1h1-rdZu4-duYgm96Sg52ion)

Posteriormente instalamos apt-transport-https para habilitar el soporte de HTTPS de nuestro administrador de paquetes apt.

![](https://lh4.googleusercontent.com/djMMSSB60oj0iuaiBYPLnopLaiF3GCqborlQZfyxuGCj5et1DGGaML7NGtR_Xl4QoUdH4Nekcngf6I6rt_lO2EwARNmWO6dAKlyQ0xcbrttJSsiUuhBTgXVHCyffilpFleZgM3WP)

Ahora se agregan los repositorios de elastic en el archivo /etc/apt/sources.list.d/elastic-7.x.list.

![](https://lh3.googleusercontent.com/lJCYObvii4fD5sv9hpoGfw-yXsH03MJ8-mXl19ZMfM0jUJRphfGu9f5ilbY2hmn_tY1InE7nmhDc9_owPD4GwQ83FbduIn2ZpN1bO-WiAhOA06RgpARTr4VrpiGlIyY7EuRVEbTf)

Luego actualizamos la base de datos del administrador de paquetes y procedemos a instalar elasticsearch.

![](https://lh5.googleusercontent.com/nCDF_KEffSBkrvJyqsZWlvXQTLtGhLxVHjOzKdb7hYdhsmjkf6SXUSHIRGAHmF45JdDqaKo7MAurQr3d7PJbj9uRTFtt1iqQGOCc1Yw_kxk8s7Mox5icwUDnUXeE8hU0tAZ5FoFJ)

Una vez instalado ejecutamos systemctl daemon-reload para recargar los archivos de configuración de systemd y generar nuevos unidades de servicio, y luego habilitamos el inicio automático de elasticsearch.

![](https://lh3.googleusercontent.com/eb61IHSfBBS5AGJyi5sl326oiua7CatLvNz2qYbynmRdC24LS_sA1QYSGsmavUjHLpO_PQQfWcoXESClAbfBZBNT1mtYhkmYugNJzsOuCnAic0tI9xuWxKR0tCUPBMbKShATTvWl)

Ahora abrimos el archivo de configuración /etc/elasticsearch/elasticsearch.yml con algún editor de texto.

![](https://lh6.googleusercontent.com/mVKC4cIrZCZuKRQpm3ptm_hQg60TrBX_Dzz_IDULKA8j1lNWcwZLbbIl4PZC7GvWwGn3FXa2xwnMzWYOy-PlRs6s6l4Dc6aYXHFDsquj2QSD5AHLfflGtepUxEN9jUQ3W_iYBP3W)

Asignamos un nombre de cluster nuevo o alguno ya existente al cual deseamos unirnos. En este caso es un nuevo cluster. Además, asignamos un nombre al nodo de elasticsearch.

![](https://lh4.googleusercontent.com/hCCh11O9G7MsMLHhadPL8GZw0-uocRCG1ugKEsY7g3NvLGxtmcWhRJZNX2jsZFr0jXG89ywQPBR4RR3yRINPUoDa4YLhX902sg69_53BISk1O_Rn1pCFqFWGCasfY9m2DIiTuKYH)

Luego establecemos una dirección IP que identifique a nuestra instancia de elasticsearch.

![](https://lh4.googleusercontent.com/HHa1dHnlpk8XJAU-LdrEFxE_6XIJoZg4_LqkyWTU1WrgDmzPZCWlpipCsAwAzAP5YznccojlGcnbigmpz3hbWimc22kp9ViquwPykfc8iNwzPtQ8lQtDeJ3dcrWWL643EUe-6edc)

Lo siguiente es indicar la lista de hosts que pertenecen al cluster y que tendrán comunicación con nuestra instancia de elasticsearch. Debido a que se creará una segunda instancia con la dirección 10.10.10.25, debemos indicar su dirección IP.

![](https://lh4.googleusercontent.com/EJOnky762A-elFg_rkPv5UT-V4tY75WANXZhVfWD_UXl4i_V5wDbJnwIM3maRlq4hfAvjMNxycb-LBGp5F49g762hce4NcH6GsZxfAja4TaqaV75Fu6rL12tQCOR7OvTjwZGF2Nd)

Ahora debemos indicar un listado de nodos que estarán disponibles para asumir el rol de nodo master. En este listado pueden indicarse sólo un subconjunto de nodos pertenecientes al cluster.

![](https://lh4.googleusercontent.com/T0tbtgdOKYyAQFVtPAFQ5iV5hwlAOgq8GC5tBp6SnB7kqrnb_J3Q9ZNxxkAYohEkR3elsVBSbt1K7rywhrMmg7UFWtyv-rxUU7W_CY9bbEUaKzl-f2Za1VDb7xcYB_Nml_n_Rwnp)

Posteriormente iniciamos el servicio de elasticsearch.

![](https://lh5.googleusercontent.com/4W0bHn7z6GLQHbmnrDbC0SXB1I_B85UrHbaSSlyI1rmzM1gZxbnhSUJbNIM0rdlEx4F26c0G3ZGq1Fhe-sSe5bGRzxYjrVSkjt6bmh6_TeotLA7J0Pca9RjY18jtC_bDs77cPiIk)

Finalmente verificamos que nuestra instancia se encuentra en funcionamiento.

![](https://lh3.googleusercontent.com/Z1m9rfWD2CjcotniAFs2zGp-NF7vvFSqouvnd2HnpNTyyjvoJFWCBkopHrOxHjPIWR0yjlIPY9BgIXW8YVKiRuIBqfGZ88FsMYoo9dECrNbKfvq8-uKXBe_eHRWneupff1snzmDf)

El proceso de instalación y configuración para la segunda instancia de elasticsearch es el mismo, simplemente se debe indicar un nombre de nodo distinto (es02.aafa.local) y su correspondiente dirección de host (10.10.10.25), además, indicamos que deseamos comunicarnos con la primera instancia (10.10.10.24).

## Logstash<a name="id403"></a>

Iniciamos por descargar gnupg con

`sudo apt install gnupg`

Los componentes de Elastic Stack no están disponibles en los repositorios de paquetes predeterminados, pero pueden instalarse con apt una vez que se agregue la lista de fuentes de paquetes de Elastic.

Todos los paquetes de Elastic Stack están firmados con la clave de firma de Elasticsearch. Así que agregaremos la lista de fuentes de paquetes de Elastic y la firma.

Importamos la clave de GPG pública de Elasticsearch en APT:

`wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -`

Ahora agregamos la lista de fuentes de Elastic al directorio sources.list.d

`echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`

Usamos

`apt update`

para que se pueda usar el repositorio recién añadido

![](https://lh6.googleusercontent.com/GA0ELVFkP_cSwoFC6m4lLO5efRt1KfwuOnwLukm3kmKBD-xIbQvpg5j8xWgZBnWK7oDvMk2RfLOqUUQ9EGjUg7XidL7FYl2mUim8yEtvR2OhJyTzO-KM65tBI8pHHSHJi_vJyWiT)

Ahora podemos instalar logstash con apt

`sudo apt install logstash`

En este momento ya se tiene logstash instalado,pero debemos configur de donde recibirá logs, cómo será el filtrado y a donde los mandará, así que creamos el archivo de configuración /etc/logstash/conf.d/test.conf donde definiremos los siguientes parámetros

Con input se definirá el plugin que alimentará a logstash, en este caso será beats. Posteriormente, para este plugin, definimos el puerto sobre el que escuchará y a que host(0.0.0.0 es “todos”)

![](https://lh3.googleusercontent.com/QP5gEh63HhANMvua-9u8T4rmQMIo7NmQhCnu0ynSbJlpviFh4LFQz4cl8eEibF5xqR9JpS1VqK2BNi4tLfNL5fON8Gd3xAQCl1GBEVSn3hO-chZDQQiEILipsIQSclj0qQosVoRP)

Ahora definimos un filtro

![](https://lh6.googleusercontent.com/JpTgJzjl0p7evQoldYfGelevdMv7XFatQ751FdR1rICytpDpZdF8p6DB-LtF-4Wf-FyVi4ytr1zfHtwPHK2DpFqq9kBEo0GjkSHA22DUAb1Sn_rGPy249p8QwIIeYA3v-Gly3VTl)

Este filtro toma los eventos auth y error de los logs de apache y los manda a donde le especifiquemos en la siguiente etiqueta.

![](https://lh5.googleusercontent.com/7tXTEnSDJIu07dE_4GVkrI9ZPMtrOkmtSN-nUw7Mk6_sKswOd8eWFEdGLGgS9ppkblsrkJ_2FBadDqOb5DLOQXhftv358BXm4n2zW9eSd-JjH5Si1FgK1TEmN2mlnBoKh1ZpvSOH)

Aquí definimos que mandaremos los logs a una instancia de elasticsearch donde el hosts el “localhost” , aunque aquí podría ir la ip de un host remoto y cuyo nombre será la versión de apache y la fecha.

Una vez guardado el archivo, probamos que no haya errores de sintaxis o de parámetros inexistentes con

`sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t`

Lo cual debería regresar un “ok” como señal de que no hay errores, así como el directorio sobre el que va a escribir

![](https://lh6.googleusercontent.com/PSfEe_Q5fRI6haxkNVAAiuE8XrCNnpiwwBHv2TESzeMAdmZ4MCYq04U6KQzBlGCy0uMYV-7r3PDSC2rpdGyzVaIgpBtYhaHECxgic4sHaUBX7_lletti4s6CkiG4rEY2CklxEQhb)

En este momento tenemos un logstash funcional.

## Kibana<a name="id404"></a>

Es una aplicación gratuita y abierta que permite a los usuarios buscar datos en índices de Elasticsearch y luego visualizar los resultados a través de opciones de gráficos estándar o apps integradas. Los usuarios pueden elegir entre diferentes gráficos y filtrar segmentos específicos de datos. Una vista de dashboard combina estos elementos visuales para compartirlos a través del navegador y brindar vistas analíticas en tiempo real de grandes volúmenes de datos.

Primero descargamos gnupg

![](https://lh5.googleusercontent.com/PyGiEkzFTkiwWkMyEYM4DQv6RqDMFzG_SeBcclpWA06tY8pPpnwbhHDDkJe-skP-obzrGq88wYsJCpJbH1ujcnL3KE3m2gpNBZ50xZtgpcKOT8usAZqpEQJKgHEpgoAGaEVdWXSY)

Después descargamos la llave PGP de Elastic.

![](https://lh4.googleusercontent.com/D6LYNcTfv1noi9kh8ortUiWnBkkxvH_nMOS5dRh-1PW5flu3otrabZE-5BTsc4dreV2Yy6wns5_Wkz9GUqjEfF_3PZbjecbyfTGG1uXedBVa7Rn-vgLJ8r9dP8oeCkkZmOdOacyy)

Antes de proceder la instalación del servicio, se debe instalar el paquete apt-transport-http

![](https://lh4.googleusercontent.com/qX0plPgTYAOKzlCdaxMQHhFlutaLSLFgLjJ_xFB8CZTNTQWdjw3dC7-EHeYVD6IQueuHnZOQ3Q0D7dnd71Qq-HOVKhrTyvWow2Yxz9LwBtWvtzap_teKpAO2DTVkV8qBF0c88mx2)

![](https://lh3.googleusercontent.com/i-PsLgJeQnw1I5Vt1Oos-KfP3MCIjklV5TFzESesCTINzI41qBH2Kj7P7O-i4y1Fec4ibg5cNWE1_BKqIXOSJxTCDlXVzkZPPG7X9-vNDXI8YmYdpMk3yVqgEPuhguFVP-gtlLEC)

Se actualizan los paquetes del manejador.

![](https://lh4.googleusercontent.com/MeRcTO3irCFNDWXEsZpHxv_SaRCYlM30eg7H_Y7YCEsNTHRiJC8Bvr7VSSvOG8KIX3WV9bAy5rUKWJ88mwIYxEobhEIcL52-ZVwuJA_vlTVQ57eShkLcYTS5hspTwQAr5HHLYMm3)

Instalamos Kibana.

![](https://lh6.googleusercontent.com/9vCwW83vv0_TJ4FlTPEH_RO4fXn7feqZooOfAhS1chgBe3bvohZ02-A_uDlxOvsOrCNcBUlc782suR0TRQgKHS-dtRRvCYEv1_Lg-aU7x6n1dPPys1gognqpw2A2hCoiMpvpXjzI)

Para configurar que Kibana inicie sin intervención después de un reinicio, se ejecutan los siguientes comandos.

![](https://lh3.googleusercontent.com/tBsKoxgu3cyEXoPo-19cYn3L0Ia8uNjCQQAlSo-I3qBuNXsusQKUbI7yOq-0XP9DHMVJW302LynVzcJGkmdUR9vbgtUiszstTrRU76jVZT_fIjHevKrk3auH2CFbR-f3ae_M-syc)

Revisamos el estatus del servicio, donde se puede observar que está apagado, se inicia y se checa nuevamente el estado de este.

![](https://lh5.googleusercontent.com/6OVAjvMxCpmuxK254j4hPzTo5yRS711tTbEkBT90rod-rA7ruFlWEd60P8UUOxKtwe3JgRWr7tbA36QWQvmRrgHta0l1QbvV01eVfjMwY-yVpsGi3jEk8vQR6Edwkx1gtUiA8D8d)

Revisamos el archivo de configuración, ubicado en /etc/kibana/kibana.yml. Para permitir conexiones remotas, editamos la línea server.host, en este caso ponemos 0.0.0.0. En cuanto al servidor Elasticsearch, la línea a editar es elasticsearch.hosts, donde se agregan las IP de los servidores.

![](https://lh3.googleusercontent.com/oHdkiO1IMeBsovBxwr-pG2RypnmI0D5X1Xi5Bg6SNwn6YUyzUPoeZ-q9gQaO7bPgUY1vV36o7qTaaN4J6ZvhdsXQZizSCnUnH5AdDB5GwikQ_iz8PzRKn5a1hCwwUKaaz_14Xd46)

Reiniciamos el servicio una vez se ha configurado.

![](https://lh5.googleusercontent.com/-vAKNP-WMYkPCAf7o62mMy7kn9QMMmqr2J_NfLo2zmgQFummUM9Fr8VcTrktmfFtPL4lTJy59cee-qJM1gs8QW-mhJlyOYwjXcnkrNWtzYitgj1wXIb9c9MvO8oRUn2tsF25O92f)

Ahora se revisa en el navegador, donde se visita [IPKibana:5601](http://ipkibana:5601).

![](https://lh6.googleusercontent.com/tHZGAKNG7AIwZ_ILK4gL3VJA7h11UPq15NeOx-xScfcYjWJcVZBeDraVT5D326w34K9tz3hmjwZTrEMN5et0p_hEBry0aUClS69hYkl1I9TmMx0lWRjD-ymHSMPAQArHzZ7gSQTb)



# Pentest<a name="id5"></a>

## Resumen ejecutivo<a name="id51"></a>

El proceso con el cual se realizó la prueba será descrito de manera general a continuación:

-   Revisión de los puertos abiertos en el objetivo con el fin de relacionarlos con servicios
    
-   Revisión de los servicios encontrados en busca de vulnerabilidades
    
-   Explotación de las vulnerabilidades encontradas
    

Con base en lo anterior, reportamos puntos a favor y en contra del sistema.

### Puntos positivos<a name="id511"></a>

Los sistemas cuentan con solo los servicios indispensables para su funcionamiento y monitoreo, lo que evita que existan varias ventanas de oportunidad para atacantes.

Los servicios encontrados no presentan vulnerabilidades críticas o altas.

### Puntos negativos<a name="id512"></a>

Los servicios son vulnerables a ataques de fuerza bruta. Esto representa un riesgo medio ya que, de sufrir un ataque exitoso, se podría comprometer la triada de la seguridad de la pila.

## Introducción<a name="id52"></a>

### Objetivo<a name="id521"></a>

El objetivo de esta prueba es encontrar vulnerabilidades en la pila ELK que pudieran ser explotadas una vez que se haga pública

### Restricciones<a name="id522"></a>

No hacer modificaciones sobre la pila

### Alcance<a name="id523"></a>

La prueba se limita a las máquinas que conforman la pila

### Antecedentes<a name="id524"></a>

Las maquina a evaluar forman una pila ELK, lo que se traduce en que los servicios de Elasticsearch, Logstash y Kibana estarán presentes por lo que ya se tiene información relevante a priori.

  
  

### Herramientas usadas<a name="id525"></a>

-   nmap: Programa que sirve para efectuar escaneo de puertos,versiones de los servicios en estos e incluso el sistema operativo del host.
    
-   Burpsuite: Plataforma integrada para realizar pruebas de seguridad de aplicaciones web.
    
-   Metasploit: Proyecto de código abierto para la seguridad informática, que proporciona información acerca de vulnerabilidades de seguridad y ayuda en Pentesting
    
-   Programación
    
-   Nessus: Software de escaneo de vulnerabilidades
    

  

## Vulnerabilidades<a name="id53"></a>


| Description |Nombre   | CVSS     |
| :---        |    :----:   |          ---: |
| SSH es vulnerable a ataques de fuerza brut      | Brute-force/Dictionary Attack       | 7.3   |
| Elasticsearch es vulnerable a ataques de fuerza bruta   | Brute-force/Dictionary Attack        | 5.3     |
| Kibana es vulnerable a ataques de fuerza bruta   | Brute-force/Dictionary Attack       | 6.5   |
| Extracción de información de servicios   | N/A     | 0.0   |

  

## Hallazgos<a name="id54"></a>

Comenzamos por lanzar un escaneo a uno de los servidores de Elasticsearch. Dado que los 3 servidores de Elasticsearch tienen los mismos servicios y configuración, solo bastará hacer pruebas en uno (el principal)

![](https://lh6.googleusercontent.com/Ni3vJ3Kv41s9EDsqmWuaqfPzxmNodK7HUZ9FvkXSztjMqRE_Xor3h3R-O4ErLM0Fbc19iH8VCtF7wanMbJGOmGq4j-9gLQfVsOmBTFCKvkb3J08E_IKb1-AaaE8oGHOKwyABBDV8)

El puerto 22 está abierto, lo que indica que hay SSH en las máquinas Elasticsearch y, como era de esperarse, nmap señala que el puerto 9200 está abierto pues ahí escucha Elasticsearch,pero nos indica que la autenticación está activada y que las conexiones por HTTP no son válidas, esto es indicador de que el servicio tiene activado HTTPS.

Para comprobar la salida de nmap, mandaremos peticiones GET al servidor para ver si logramos extraer algún tipo de información

El primer paso es comprobar si puede obtener la versión o información acerca de los usuarios

![](https://lh5.googleusercontent.com/Ywjw9pqrz6har-9uWZwIC9jHTqTG2c4jWSCavfRDROqO5oUBM4Y-FoZgv6XdzKywah7fWZ0OLgmcLlc49Zcznr-l4xL0SZuqMlR0xSazFAzGpJJr5jmfCnMEkwfWqYKwrApYRFB0)

La respuesta vacía indica que el HTTP no es posible. Así que pasaremos a usar HTTPS

![](https://lh4.googleusercontent.com/OLzu8oyZe9GKF3vXXC6nO1fZLEWAWE3tfVtjP7jBoo3VlZbyzIxvMpvCGBR3wRxeQnRNf5lr0MtlLkz8y3E5_bDs1p0pz26fLCuMA5gjT0A1H_1PQbUWuaYZPBGWcwHzOtPKC0Lv)

Efectivamente, se está usando HTTPS, así que ahora usaremos la bandera -k para evitar esta advertencia debido a los certificados autofirmados

  

![](https://lh5.googleusercontent.com/IjagSqWBmXFHJ1EDCoPATYVOaL0jppxhT449n9B_Z3uHy09jwzB30LyPr7t9AWruIPU2MDJygmnvQYuy-SKVR20hDpAhYuoXWpvdz_d4rSfQmrJxYICsuGFmRox-wRGHaj0RfMpA)

La respuesta indica que hay autenticación de por medio, justo como nmap nos indicó. Así que ahora se procederá a intentar un ataque de fuerza bruta sobre el servicio con un pequeño script de bash que hemos programado para este análisis. El script mandará peticiones al servidor intentando obtener un usuario y contraseña válidos.

![](https://lh6.googleusercontent.com/AxUsn591cBdkM8FK5KvFYZat906tV8FECn1-LVTt3-ujyjdkiEV6EvBq0m_AkiZgwjr12MIwgQXM4dXB3QAA8yheTJY98iuIENrvShLdmgamGF2m16sQe1qpxRJsEeepwVxlfsDH)

![](https://lh3.googleusercontent.com/ZnLSRInz2M3Cys7SWkkUV6zzzapTMxE8Sx3UpD2OkC4neVY2rY0_f_H5PCqefCbv9_5T52p26vX0X-3t-Fu6iLao9D-B63MZNZ9aUCWbBJ8FFW_VLBF0bNdi29mrucNxI7fcjvzw)

Dado que el servidor en ningún momento dejó de recibir nuestras peticiones, sabemos que no hay un servicio protegiendo de este tipo de ataques, por lo que los servidores Elasticsearch a analizar son vulnerables a ataques de fuerza bruta/diccionario sobre el mismo servicio de Elasticsearch.

Ahora procedemos a analizar SSH sobre la máquina Elasticsearch principal. Dado que toda la infraestructura ELK comparte configuración de SSH, solo bastará hacerlo sobre esta máquina.

Intentaremos enumerar los usuarios del sistema por medio de la técnica de paquetes malformados a la cual algunas versiones de SSH son vulnerables. Nos apoyaremos en metasploit

![](https://lh6.googleusercontent.com/W9-hoZebIVR8sI9F2PKoydwDaH3nEI2MLUIBGqoo6jLmjXKHguonbxAWSJrODxkELh2r9aVMyoWmuOeWrRQVxQ79oEC__ydRRNSrkxI8_Y2Aivkw7uG8B9RZUWKoy3tGzhHJtf05)

Dado que se están registrando falsos positivos, no podemos asegurar que ssh sea vulnerable a este tipo de ataques.

Ahora verificaremos si el servicio es vulnerable a ataques de fuerza bruta/diccionario. De la misma forma que el servicio anterior, si la máquina sigue recibiendo nuestras peticiones a pesar de ser muchas en poco tiempo, sabremos que no están protegidos contra este tipo de ataques.

![](https://lh3.googleusercontent.com/WhPsq_bVqW3zNe_iA630qiJ0n_nNmXEPUtU0v7siX8vFjbvZYXNsvcwDcmP_BxvahDtHFcBtZB3i5Hrza8gu6VdqEBEZZwT8cJs0Vi02Fal9jH2cGW8SS76QyLH-rKmZavNf7K5y)

Podemos notar que el servidor siguió tomando nuestras peticiones a pesar del intervalo de tiempo, por lo que es vulnerable a este tipo de ataques.

  

Por último haremos pruebas sobre el servidor de Kibana. Ya sabemos que tiene un servicio de ssh vulnerable a ataques de fuerza bruta/diccionario pues todas las máquinas del servidor comparten configuración de ssh, así que usaremos el escáner de Nessus para identificar algún otro servicio o ventana de oportunidad.

![](https://lh3.googleusercontent.com/ImPJNffkCjvgxbDY-buAXwlujLkAv4AEh2k49uaCva1JMvaSxWM5LWsl8FAklfV6z7GTKFexsn4VZQiyDYBHZfEs-1iomD0FoOeaRgDplFQBZQXVX45PnCypGhLX5YU8Hf3Kmr1Z)

En cuanto al servidor, no hay vulnerabilidades importantes detectadas. Las vulnerabilidades medias que se muestran son debido a los certificados auto firmados y las otras son información que se pudo extraer, pero que no consideramos de peligro.

Antes de intentar algo sobre el servidor web, se usará el escáner de vulnerabilidades web de Nessus para cubrir las espacio de búsqueda.

![](https://lh6.googleusercontent.com/jLY8rwNTCOCzZzp-eTldm0YnUffOpOXcNjgDlf7WB1I5YfwJhXiAVLrE4Bm9RN-4iNc_gcsFikh3FG6EjjYd2CySsHuUNaUtfPJHN1ZVeceiW7mpRVXsCA0DjPI8LSzlQRp4moyn)

De nuevo, no se indican vulnerabilidades que explotar. Así que ahora entraremos de forma manual para verificar que no hay nada que explotar.

![](https://lh5.googleusercontent.com/mgb47JhxSOOIOZZC2EuJp_eyT4MVwQcIaosA6rBt9pi1esmBd8GGY99rmTpGi2uHQiP40jFJogHZwdNYZCnIHOknUi71QH8WJsbOeiMZQy9oSKlP9X0gNdGfxN50RqW_UodpiY9e)

Capturamos la petición que se acaba de mandar con Burpsuite

![](https://lh6.googleusercontent.com/liKxvB2taxWSRMUcV_v_Sp0prn5c29mojp8UeFLFZkaZVIv_T8bCXiKMQ_O7ue2t0mXkfOm5GXLwbeWOEDfT5uET7guixllCH6-ie_HxkuQayWQ7-nKcDAh_07NTpkaprf5GoyyW)

La respuesta indica que no se pudo autenticar con esa combinación de usuario y contraseña, así que podríamos analizar respuestas de muchas peticiones para identificar una combinación de usuario y contraseña válida. Entonces procederemos a hacer un ataque de diccionario sobre Kibana.

  

![](https://lh4.googleusercontent.com/bMmWcjf6qZeahJlvOugzw9EiRHTVkS5jXQbH8KaGzgeBcRd7-X7GB3VTJesBog5QiItlICFcULNP6Z3okGwkIwvYJwWMcEgNDWISfA4cOcqHFssSrcit3NFATOAsZHxMjq7FRrM9)

Aquí se muestra como empezamos a configurar el ataque y a continuación la ejecución

![](https://lh5.googleusercontent.com/fWQOO2UBfZ7LmMEsHrwJRXJZE99FAwubTFpEJ-nln1E_ESGoMAMnaYkgGxle6rhswsV7n2IVSzQoaFzEqXnL7FjrUroyJB5nQpMQQJRooExh_QK7SpHypuCL7pAu3vyVZ2D7ls9j)

Notemos que el ataque puede seguirse ejecutando sin problemas, por lo que el servidor es vulnerable a este tipo de ataques.

  

## Recomendaciones<a name="id55"></a>

-   Instalar un servicio como Fail2ban para evitar este tipo de ataques.
    
-   Cambiar los puertos comunes usados por dichos servicios
    
-   No escuchar en todas las interfaces
    
-   Usar certificados en lugar de contraseñas para autenticar en SSH
 

# Conclusión.<a name="id6"></a>

El proyecto ayudó a reforzar los conocimientos sobre servicios y redes que se adquirieron durante el Plan de Becarios, en particular sobre infraestructura de Windows, ya que no se volvió a implementar un servicio sobre este sistema operativo después del primer módulo. Así mismo, se adquirió conocimientos nuevos sobre herramientas como lo son Amavis, Spamassassin, FortiGate, GNS3 y Cerebro, entre otros. Además, nos permitió adquirir conocimientos referente a la realización de un hardening sobre sistemas operativos Debian y CentOS, lo cual nos parece muy útil ya que es necesario implementar una línea base sobre aquellos equipos que conforman nuestra infraestructura de trabajo.

Por otra parte, se logró un aprendizaje significativo respecto al funcionamiento, configuración, instalación y administración de una pila ELK.

Fue una gran experiencia de aprendizaje.

# Bibliografía.<a name="id7"></a>

[3.7.1 Spamassassin [Servidor Debian]. (s/f-a). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/BCdZ) [https://servidordebian.org/es/buster/intranet/antispam/spamassassin](https://servidordebian.org/es/buster/intranet/antispam/spamassassin)

[3.7.1 Spamassassin [Servidor Debian]. (s/f-b). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/ZNBK) [https://servidordebian.org/es/buster/intranet/antispam/spamassassin](https://servidordebian.org/es/buster/intranet/antispam/spamassassin)

[5.3.1 amavisd-new [Servidor Debian]. (s/f-a). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/TDGF) [https://servidordebian.org/es/buster/email/filtering/amavisd_new](https://servidordebian.org/es/buster/email/filtering/amavisd_new)

[5.3.1 amavisd-new [Servidor Debian]. (s/f-b). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/mB6S) [https://servidordebian.org/es/buster/email/filtering/amavisd_new](https://servidordebian.org/es/buster/email/filtering/amavisd_new)

[5.3.3 Filtro Anti-spam [Servidor Debian]. (s/f). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/cJ1s) [https://servidordebian.org/es/buster/email/filtering/amavisd_new_spamassassin](https://servidordebian.org/es/buster/email/filtering/amavisd_new_spamassassin)

[Add and remove nodes in your cluster. (s/f). Recuperado el 5 de febrero de 2021, de](http://paperpile.com/b/Q4Pnnj/wfrj) [https://www.elastic.co/guide/en/elasticsearch/reference/current/add-elasticsearch-nodes.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/add-elasticsearch-nodes.html)

[Bearnes, B. (2016, enero 27). How To Protect SSH With Fail2Ban on CentOS 7. DigitalOcean.](http://paperpile.com/b/Q4Pnnj/RStP) [https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-centos-7](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-centos-7)

[Bootstrapping a cluster. (s/f). Recuperado el 5 de febrero de 2021, de](http://paperpile.com/b/Q4Pnnj/8aLf) [https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-bootstrap-cluster.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-bootstrap-cluster.html)

[Boucheron, B., Drake, M., & Heidi, E. (2019, julio 22). How To Create a Self-Signed SSL Certificate for Apache in Debian 10. DigitalOcean.](http://paperpile.com/b/Q4Pnnj/R5dp) [https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-debian-10](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-debian-10)

[Building a Transparent Firewall with Linux, Part V. (s/f). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/bjeV) [https://www.linuxjournal.com/article/10929](https://www.linuxjournal.com/article/10929)

[Cezar, M. (2017, octubre 12). Install a Complete Mail Server with Postfix and Webmail in Debian 9.](http://paperpile.com/b/Q4Pnnj/1okQ) [https://www.tecmint.com/](https://www.tecmint.com/)

[Cookbook. (s/f-a). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/yF1k) [https://docs.fortinet.com/document/fortigate/6.2.0/cookbook/954635/getting-started](https://docs.fortinet.com/document/fortigate/6.2.0/cookbook/954635/getting-started)

[Cookbook. (s/f-b). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/k316) [https://docs.fortinet.com/document/fortigate/6.2.0/cookbook/954635/getting-started](https://docs.fortinet.com/document/fortigate/6.2.0/cookbook/954635/getting-started)

[Erkec, E., & View all posts by Esat Erkec →. (2020, enero 16). How to install SQL Server Express edition.](http://paperpile.com/b/Q4Pnnj/jGIo) [https://www.sqlshack.com/how-to-install-sql-server-express-edition/](https://www.sqlshack.com/how-to-install-sql-server-express-edition/)

[Filebeat quick start: installation and configuration. (s/f). Recuperado el 5 de febrero de 2021, de](http://paperpile.com/b/Q4Pnnj/3FPr) [https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html)

[Getting started with the Elastic Stack. (s/f-a). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/Ii4k) [https://www.elastic.co/guide/en/elastic-stack-get-started/7.10/get-started-elastic-stack.html#install-beats](https://www.elastic.co/guide/en/elastic-stack-get-started/7.10/get-started-elastic-stack.html#install-beats)

[Getting started with the Elastic Stack. (s/f-b). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/QZKK) [https://www.elastic.co/guide/en/elastic-stack-get-started/7.10/get-started-elastic-stack.html#install-kibana](https://www.elastic.co/guide/en/elastic-stack-get-started/7.10/get-started-elastic-stack.html#install-kibana)

[Getting started with the Elastic Stack. (s/f-c). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/cHcO) [https://www.elastic.co/guide/en/elastic-stack-get-started/7.10/get-started-elastic-stack.html#install-elasticsearch](https://www.elastic.co/guide/en/elastic-stack-get-started/7.10/get-started-elastic-stack.html#install-elasticsearch)

[GNS3 Talks: Integrate Windows Virtual Machine with GNS3 = GNS3+GNS3 VM + Windows 10 VM + Cisco. (2017, enero 28).](http://paperpile.com/b/Q4Pnnj/YHg2) [https://www.youtube.com/watch?v=Ot6nqrNid3M](https://www.youtube.com/watch?v=Ot6nqrNid3M)

[How To add DNS A/PTR Record in Windows Server 2019. (2019, noviembre 15).](http://paperpile.com/b/Q4Pnnj/VKy8) [https://computingforgeeks.com/how-to-add-dns-a-ptr-record-in-windows-server/](https://computingforgeeks.com/how-to-add-dns-a-ptr-record-in-windows-server/)

[How To add DNS Forward Lookup Zone in Windows Server 2019. (2019, noviembre 11).](http://paperpile.com/b/Q4Pnnj/GYVo) [https://computingforgeeks.com/how-to-add-dns-forward-lookup-zone-in-windows-server/](https://computingforgeeks.com/how-to-add-dns-forward-lookup-zone-in-windows-server/)

[How To add DNS Reverse Lookup Zone in Windows Server 2019. (2019, noviembre 13).](http://paperpile.com/b/Q4Pnnj/yJlK) [https://computingforgeeks.com/how-to-add-dns-reverse-lookup-zone-in-windows-server/](https://computingforgeeks.com/how-to-add-dns-reverse-lookup-zone-in-windows-server/)

[How to deploy FortiGate Firewall in GNS3. (2020, marzo 17).](http://paperpile.com/b/Q4Pnnj/Q5qt) [https://www.gns3network.com/how-to-deploy-fortigate-virtual-firewall-in-gns3/](https://www.gns3network.com/how-to-deploy-fortigate-virtual-firewall-in-gns3/)

[How to Setup ELK Stack on Debian 9 / Debian 8 - ITzGeek. (2017, septiembre 14).](http://paperpile.com/b/Q4Pnnj/EYr4) [https://www.itzgeek.com/how-tos/linux/debian/how-to-setup-elk-stack-on-debian-9-debian-8.html](https://www.itzgeek.com/how-tos/linux/debian/how-to-setup-elk-stack-on-debian-9-debian-8.html)

[Install and Configure DHCP Server on Windows Server 2019. (2019, octubre 12).](http://paperpile.com/b/Q4Pnnj/DdJy) [https://computingforgeeks.com/how-to-install-and-configure-dhcp-server-on-windows-server/](https://computingforgeeks.com/how-to-install-and-configure-dhcp-server-on-windows-server/)

[Install and Configure DNS Server on Windows Server 2019. (2019, noviembre 7).](http://paperpile.com/b/Q4Pnnj/Z1es) [https://computingforgeeks.com/install-and-configure-dns-server-in-windows-server/](https://computingforgeeks.com/install-and-configure-dns-server-in-windows-server/)

[Install Elasticsearch with Debian Package. (s/f). Recuperado el 5 de febrero de 2021, de](http://paperpile.com/b/Q4Pnnj/BDmt) [https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)

[Install GNS3 with GNS3 VM with routers and switches| CCNA / CCNP Practical Labs. (2020, junio 16).](http://paperpile.com/b/Q4Pnnj/Vzxp) [https://www.youtube.com/watch?v=c1Lr_xOqaIA](https://www.youtube.com/watch?v=c1Lr_xOqaIA)

[Install Kibana with Debian package. (s/f). Recuperado el 4 de febrero de 2021, de](http://paperpile.com/b/Q4Pnnj/4y69) [https://www.elastic.co/guide/en/kibana/current/deb.html#deb-repo](https://www.elastic.co/guide/en/kibana/current/deb.html#deb-repo)

[Juell, K., & Heidi, E. (2019, julio 10). How to Install Nginx on Debian 10. DigitalOcean.](http://paperpile.com/b/Q4Pnnj/E7BG) [https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-10](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-10)

[Logstash Introduction. (s/f). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/vL3w) [https://www.elastic.co/guide/en/logstash/7.10/introduction.html](https://www.elastic.co/guide/en/logstash/7.10/introduction.html)

[Mercado, J. A., & Ver todas las entradas por Juan Andres Mercado →. (2017, octubre 18). How to Install Kibana with Debian Packages.](http://paperpile.com/b/Q4Pnnj/YtQu) [https://juanmercadoit.com/2017/10/18/how-to-install-kibana-with-debian-packages/](https://juanmercadoit.com/2017/10/18/how-to-install-kibana-with-debian-packages/)

[Node. (s/f). Recuperado el 5 de febrero de 2021, de](http://paperpile.com/b/Q4Pnnj/WKfg) [https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-node.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-node.html)

[Orovengua, J. (2013, abril 11). LinuxParty.](http://paperpile.com/b/Q4Pnnj/62IT) [https://www.linuxparty.es/29-internet/8661-crear-un-puente-de-red-o-bridge](https://www.linuxparty.es/29-internet/8661-crear-un-puente-de-red-o-bridge)

[Protegerse de ataques de fuerza bruta con fail2ban. (s/f). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/qSUi) [https://www.ionos.mx/ayuda/seguridad/servidor-dedicado/protegerse-de-ataques-de-fuerza-bruta-con-fail2ban/](https://www.ionos.mx/ayuda/seguridad/servidor-dedicado/protegerse-de-ataques-de-fuerza-bruta-con-fail2ban/)

[Rodríguez, S. B. (2019, octubre 24). Servidor de correo – Instalación y configuración Amavis, Clamav y Spamassassin (parte 11/15).](http://paperpile.com/b/Q4Pnnj/mpWZ) [https://blog.tiraquelibras.com/?p=667](https://blog.tiraquelibras.com/?p=667)

[Website. (s/f). Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/TAyH) [https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-18-04](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-18-04)

[Winlogbeat quick start: installation and configuration. (s/f). Recuperado el 5 de febrero de 2021, de](http://paperpile.com/b/Q4Pnnj/qFzA) [https://www.elastic.co/guide/en/beats/winlogbeat/7.10/winlogbeat-installation-configuration.html](https://www.elastic.co/guide/en/beats/winlogbeat/7.10/winlogbeat-installation-configuration.html)

[yombi. (s/f). yombi/Migrar-Drupal. Recuperado el 22 de enero de 2021, de](http://paperpile.com/b/Q4Pnnj/ze8O) [https://github.com/yombi/Migrar-Drupal](https://github.com/yombi/Migrar-Drupal)

