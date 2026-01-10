### **Authentication vulnerabilities**

La autenticación es el proceso de verificar la identidad de un usuario o cliente. Los sitios web están potencialmente expuestos a cualquier persona que esté conectada a Internet. Esto hace que los mecanismos de autenticación robustos sean integrales para una seguridad web efectiva.

**Hay tres tipos principales de autenticación:**

-  Algo que sabemos, como una contraseña o la respuesta a una pregunta de seguridad. Estos a veces se llaman "factores de conocimiento".
- Algo que tienemos, este es un objeto físico como un teléfono móvil o un token de seguridad. Estos a veces se llaman "factores de posesión".
- Algo que eres o haces. Por ejemplo, su biometría o patrones de comportamiento. A veces se les llama "factores de inherencia".

Los mecanismos de autenticación se basan en una serie de tecnologías para verificar uno o más de estos factores.

**¿Cuál es la diferencia entre autenticación y autorización?**

La autenticación es el proceso de verificar que un usuario es quien dice ser. 
La autorización implica verificar si un usuario puede hacer algo.

Por ejemplo, la autenticación determina si alguien que intenta acceder a un sitio web con el nombre de usuario Carlos123 es realmente la misma persona que creó la cuenta.

Una vez que Carlos123 está autenticado, sus permisos determinan lo que están autorizados a hacer. Por ejemplo, pueden estar autorizados para acceder a información personal sobre otros usuarios, o realizar acciones como eliminar la cuenta de otro usuario.

**¿Cómo surgen las vulnerabilidades de autenticación?**

La mayoría de las vulnerabilidades en los mecanismos de autenticación se producen de una de dos maneras:

- Los mecanismos de autenticación son débiles porque no protegen adecuadamente contra los ataques de fuerza bruta.
- Los defectos lógicos o la codificación deficiente en la implementación permiten que los mecanismos de autenticación sean omitidos por completo por un atacante. Esto a veces se llama "autenticación rota".

En muchas áreas del desarrollo web, los defectos lógicos hacen que el sitio web se comporte inesperadamente, lo que puede o no ser un problema de seguridad. Sin embargo, como la autenticación es tan crítica para la seguridad, es muy probable que la lógica de autenticación defectuosa exponga al sitio web a problemas de seguridad.

**Vulnerabilidades en el inicio de sesión basado en contraseña**

Para los sitios web que adoptan un proceso de inicio de sesión basado en contraseña, los usuarios se registran para una cuenta o se les asigna una cuenta por un administrador. Esta cuenta está asociada con un nombre de usuario único y una contraseña secreta, que el usuario ingresa en un formulario de inicio de sesión para autenticarse.

En este escenario, el hecho de que conozcan la contraseña secreta se toma como prueba suficiente de la identidad del usuario. Esto significa que la seguridad del sitio web se ve comprometida si un atacante puede obtener o adivinar las credenciales de inicio de sesión de otro usuario.

Esto se puede lograr de varias maneras. Las siguientes secciones muestran cómo un atacante puede usar ataques de fuerza bruta y algunos de los defectos en la protección de la fuerza bruta. También aprenderá sobre las vulnerabilidades en la autenticación básica HTTP.

**Ataques de fuerza bruta**

Un ataque de fuerza bruta es cuando un atacante utiliza un sistema de prueba y error para adivinar las credenciales de usuario válidas. Estos ataques suelen ser automatizados utilizando listas de palabras de nombres de usuario y contraseñas. La automatización de este proceso, especialmente el uso de herramientas dedicadas, potencialmente permite a un atacante hacer un gran número de intentos de inicio de sesión a alta velocidad.

El forzamiento bruto no siempre es solo un caso de hacer conjeturas completamente aleatorias en los nombres de usuario y contraseñas. Al usar también la lógica básica o el conocimiento disponible públicamente, los atacantes pueden ajustar los ataques de fuerza bruta para hacer conjeturas mucho más educadas. Esto aumenta considerablemente la eficiencia de tales ataques. Los sitios web que dependen del inicio de sesión basado en contraseñas como su único método para autenticar a los usuarios pueden ser altamente vulnerables si no implementan suficiente protección de fuerza bruta.

**Nombres de usuario de forzamiento bruto**

Los nombres de usuario son especialmente fáciles de adivinar si se ajustan a un patrón reconocible, como una dirección de correo electrónico. Por ejemplo, es muy común ver los inicios de sesión de negocios en el formato firstname.lastname@somecompany.com. Sin embargo, incluso si no hay un patrón obvio, a veces incluso las cuentas de alto privilegio se crean utilizando nombres de usuario predecibles, como administrador o administrador.

Durante la auditoría, verifique si el sitio web divulga públicamente posibles nombres de usuario. Por ejemplo, ¿puedes acceder a los perfiles de usuario sin iniciar sesión? Incluso si el contenido real de los perfiles está oculto, el nombre utilizado en el perfil es a veces el mismo que el nombre de usuario de inicio de sesión. También debe comprobar las respuestas HTTP para ver si se divulga alguna dirección de correo electrónico. Ocasionalmente, las respuestas contienen direcciones de correo electrónico de usuarios de alto privilegio, como administradores o soporte de TI.

**Contraseñas de fuerza bruta**

Las contraseñas pueden ser igualmente forzadas, con la dificultad de variar en función de la fuerza de la contraseña. Muchos sitios web adoptan alguna forma de política de contraseñas, que obliga a los usuarios a crear contraseñas de alta entropía que son, teóricamente al menos, más difíciles de descifrar utilizando solo la fuerza bruta. Esto generalmente implica hacer cumplir las contraseñas con:

- Un mínimo de caracteres
- Una mezcla de letras bajas y mayúsculas
- Al menos un personaje especial

Sin embargo, si bien las contraseñas de alta entropía son difíciles de descifrar para las computadoras, podemos usar un conocimiento básico del comportamiento humano para explotar las vulnerabilidades que los usuarios introducen involuntariamente a este sistema. En lugar de crear una contraseña segura con una combinación aleatoria de caracteres, los usuarios a menudo toman una contraseña que pueden recordar e intentan cortarla para que se ajuste a la política de contraseñas. Por ejemplo, si no se permite mypassword, los usuarios pueden probar algo como Mypassword1! O "Myp4$sword en su lugar.

En los casos en que la política requiere que los usuarios cambien sus contraseñas de forma regular, también es común que los usuarios simplemente realicen cambios menores y predecibles en su contraseña preferida. Por ejemplo, Mypassword1! ¿Se convierte en Mypassword1? O Mypassword2!.

Este conocimiento de credenciales probables y patrones predecibles significa que los ataques de fuerza bruta a menudo pueden ser mucho más sofisticados y, por lo tanto, efectivos que simplemente iterar a través de cada combinación posible de personajes.

**Enumeración de nombre de usuario**

La enumeración de nombre de usuario es cuando un atacante puede observar cambios en el comportamiento del sitio web para identificar si un nombre de usuario determinado es válido.

La enumeración de nombres de usuario generalmente ocurre en la página de inicio de sesión, por ejemplo, cuando ingresa un nombre de usuario válido pero una contraseña incorrecta, o en los formularios de registro cuando ingresa un nombre de usuario que ya está tomado. Esto reduce en gran medida el tiempo y el esfuerzo requeridos para forzar brutamente un inicio de sesión porque el atacante es capaz de generar rápidamente una lista de nombres de usuario válidos.

Al intentar forzar brutamente una página de inicio de sesión, debe prestar especial atención a cualquier diferencia en:

- **Códigos de estado**: Durante un ataque de fuerza bruta, es probable que el código de estado HTTP devuelto sea el mismo para la gran mayoría de las conjeturas porque la mayoría de ellos estarán equivocados. Si una suposición devuelve un código de estado diferente, esta es una fuerte indicación de que el nombre de usuario era correcto. Es una práctica recomendada para los sitios web devolver siempre el mismo código de estado independientemente del resultado, pero esta práctica no siempre se sigue.
- **Mensajes de error:** A veces el mensaje de error devuelto es diferente dependiendo de si tanto el nombre de usuario como la contraseña son incorrectos o solo la contraseña era incorrecta. Es la mejor práctica para que los sitios web utilicen mensajes genéricos idénticos en ambos casos, pero a veces aparecen pequeños errores de escritura. Un solo carácter fuera de lugar hace que los dos mensajes sean distintos, incluso en los casos en que el carácter no es visible en la página renderizada.
- **Tiempos de respuesta:** Si la mayoría de las solicitudes se manejaron con un tiempo de respuesta similar, cualquiera que se desvíe de esto sugiere que algo diferente estaba sucediendo detrás de escena. Esta es otra indicación de que el nombre de usuario adivinado podría ser correcto. Por ejemplo, un sitio web solo puede comprobar si la contraseña es correcta si el nombre de usuario es válido. Este paso extra puede causar un ligero aumento en el tiempo de respuesta. Esto puede ser sutil, pero un atacante puede hacer que este retraso sea más obvio al ingresar una contraseña excesivamente larga que el sitio web tarda notablemente más en manejar.



**Protección de fuerza bruta defectuosa**

Es muy probable que un ataque de fuerza bruta involucre muchas conjeturas fallidas antes de que el atacante comprometa con éxito una cuenta. Lógicamente, la protección de fuerza bruta gira en torno a tratar de hacerlo lo más complicado posible para automatizar el proceso y ralentizar la velocidad a la que un atacante puede intentar inicios de sesión. Las dos formas más comunes de prevenir los ataques de fuerza bruta son:
-
- Bloquear la cuenta a la que el usuario remoto está intentando acceder si realiza demasiados intentos de inicio de sesión fallidos
- Bloquear la dirección IP del usuario remoto si realiza demasiados intentos de inicio de sesión en rápida sucesión

Ambos enfoques ofrecen diversos grados de protección, pero ninguno es invulnerable, especialmente si se implementa utilizando una lógica defectuosa.

Por ejemplo, a veces puede encontrar que su IP está bloqueada si no inicia sesión demasiadas veces. En algunas implementaciones, el contador para el número de intentos fallidos se reinicia si el propietario de IP inicia sesión correctamente. Esto significa que un atacante simplemente tendría que iniciar sesión en su propia cuenta cada pocos intentos para evitar que se alcance este límite.

En este caso, simplemente incluir sus propias credenciales de inicio de sesión a intervalos regulares a lo largo de la lista de palabras es suficiente para hacer que esta defensa sea prácticamente inútil.

**Bloqueo de cuenta**

Una forma en que los sitios web intentan evitar el forzamiento bruto es bloquear la cuenta si se cumplen ciertos criterios sospechosos, generalmente un número determinado de intentos fallidos de inicio de sesión. Al igual que con los errores normales de inicio de sesión, las respuestas del servidor que indican que una cuenta está bloqueada también pueden ayudar a un atacante a enumerar nombres de usuario.

Bloquear una cuenta ofrece una cierta cantidad de protección contra el forzamiento bruto dirigido de una cuenta específica. Sin embargo, este enfoque no logra prevenir adecuadamente los ataques de fuerza bruta en los que el atacante solo está tratando de obtener acceso a cualquier cuenta aleatoria que puedan.

Por ejemplo, el siguiente método se puede utilizar para solucionar este tipo de protección:

- Establecer una lista de nombres de usuario candidatos que probablemente sean válidos. Esto podría ser a través de la enumeración de nombres de usuario o simplemente en base a una lista de nombres de usuario comunes.
- Decida sobre una lista muy pequeña de contraseñas que cree que al menos un usuario probablemente tenga. De manera crucial, el número de contraseñas que seleccione no debe exceder el número de intentos de inicio de sesión permitidos. Por ejemplo, si ha resuelto que el límite es de 3 intentos, debe elegir un máximo de 3 conjeturas de contraseñas.
- Usando una herramienta como Burp Intruder, pruebe cada una de las contraseñas seleccionadas con cada uno de los nombres de usuario candidatos. De esta manera, puede intentar forzar brutamente cada cuenta sin activar el bloqueo de la cuenta. Solo necesita un solo usuario para usar una de las tres contraseñas con el fin de comprometer una cuenta.


El bloqueo de la cuenta tampoco protege contra los ataques de relleno de credenciales. Esto implica el uso de un diccionario masivo de pares de nombre de usuario:password, compuesto de credenciales de inicio de sesión genuinas robadas en violaciones de datos. El relleno de credenciales se basa en el hecho de que muchas personas reutilizan el mismo nombre de usuario y contraseña en múltiples sitios web y, por lo tanto, existe la posibilidad de que algunas de las credenciales comprometidas en el diccionario también sean válidas en el sitio web de destino. El bloqueo de cuenta no protege contra el relleno de credenciales porque cada nombre de usuario solo se intenta una vez. El relleno de credenciales es particularmente peligroso porque a veces puede resultar en que el atacante comprometa muchas cuentas diferentes con un solo ataque automatizado.