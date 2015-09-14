##Descripción de los comandos de CVS 

El formato general de todos los comandos de CVS es:

	cvs [ cvs_options ] cvs_command [ command_options ] [ command_args ]

Donde:

	cvs

Es el nombre del programa cvs.

	cvs_options

Algunas opciones que afectan a todos los sub-comandos de CVS.

	cvs_command

Uno de varios sub-comandos diferentes. Algunos de los comandos tienen alias que se pueden utilizar en su lugar; los alias se indican en el manual de referencia para ese comando. Sólo hay dos situaciones en las que es posible omitir 'cvs_command': '-H cvs' provoca una lista de comandos disponibles, y '-v cvs' muestra información de la versión en sí CVS.

	command_options

Opciones que son específicas del comando.

	command_args

Argumentos a los comandos.


###Opciones globales

Las _'cvs_options'_ disponibles son:

	--allow-root=rootdir

Especifica el directorio _CVSROOT_ legal.

	-a

Autentifica todas las comunicaciones entre el cliente y el servidor. Solo tiene efecto en el cliente CVS.

	- T tempdir

Use _tempdir_ como el directorio donde los archivos temporales son colocados. Anula la configuración de la variable de entorno $ TMPDIR y cualquier directorio precompilado. Este parámetro debe especificarse como una ruta absoluta.

	-d cvs_root_directory

Utilice _cvs_root_directory_ como la ruta de acceso al directorio raíz del repositorio. Anula la configuración de la variable de entorno _$ CVSROOT_.

	-e editor

Utilice editor para introducir la información de registro de revisiones. Anula el ajuste de las variables de entorno _$ CVSEDITOR_ y $ EDITOR.

	 -H
	--help

Muestra informacion del uso del comando _'cvs_command'_ especificado.

	-r
Haga nuevos archivos de trabajo de sólo lectura. Mismo efecto que si se establece la variable de entorno _$CVSREAD_.

	-s variable=value

Establece una variable de usuario.

	-t

Traza la ejecución del programa; muestra mensajes en la pantalla de los pasos de la actividad CVS.

	 -v
	--version

Muestra la version y la información del copyright para cvs.

	-w
Crea nuevos archivos de trabajo de lectura y escritura. Anula la configuración de la variable de entorno _$ CVSREAD_.

####Añadir archivos y directorios en el repositorio

	add [-k rcs-kflag] [-m message] files...

El comando _add_ se utiliza para presentar los nuevos archivos y directorios para la adición al repositorio CVS. Cuando se utiliza _add_ en un directorio, un nuevo directorio se crea en el repositorio inmediatamente. Cuando se utiliza en un archivo, sólo el directorio de trabajo se actualiza. Los cambios en el repositorio no se hacen hasta que el comando _commit_ se utiliza en el archivo que se acaba de agregar.
El comando _add_ también resucita a los archivos que se han eliminado previamente. Esto se puede hacer antes o después de que el comando _commit_ se utilice para finalizar la eliminación de archivos. Los archivos resucitados se restauran en el directorio de trabajo en el momento en que se ejecute el comando add.

#####Opciones del comando _add_

	-k kflag

Procesa palabras clave de acuerdo con kflag. 

	-m message

Utiliza _message_ como el mensaje de registro, en lugar de invocar un editor.

####_annotate_ - ¿Qué revisión modificó cada línea de un archivo?
	annotate [options] files…

Para cada archivo en _files_, imprime la revisión principal del tronco, junto con información sobre la última modificación para cada línea.

#####Opciones del comando _annotate_

	-l

Directorio local solamente, sin recursividad.

	-R

Directorios de proceso de forma recursiva.

	-f

Utilice revisión de cabecera si no se encuentra la etiqueta / fecha.

	-F

Comentar archivos binarios.

	-r revision

Comentar archivo como de la revisión / etiqueta específicada.

	-D date

Comentar archivo como de la fecha específicada.

#####Ejemplo del comando _annotate_

	$ cvs annotate ssfile
	Annotations for ssfile
	***************
	1.1          (mary     27-Mar-96): ssfile line 1
	1.2          (joe      28-Mar-96): ssfile line 2

El archivo _'ssfile'_ contiene actualmente dos líneas. La línea _ssfile line 1_ se registró por mary el 27 de marzo. Luego, el 28 de marzo, joe añadió la línea _ssfile line 2_, sin modificar la línea _ssfile line 1_. Este informe no dice nada acerca de las líneas que han sido borradas o reemplazadas; es necesario utilizar _cvs diff_ para eso.

####_checkout_ - Mirar las fuentes para la edición

	checkout [options] modules…

Crea o actualiza un directorio de trabajo que contiene copias de los archivos de fuente especificados por _modules_. Debe ejecutar _checkout_ antes de usar la mayoría de los otros comandos de CVS, ya que la mayoría de ellos operan en su directorio de trabajo.

Los módulos son o bien los nombres simbólicos para alguna colección de directorios fuente y archivos, o rutas de acceso a directorios o archivos en el repositorio. Los nombres simbólicos se definen en el archivo _'modules'_.

En función de los módulos especificados, _checkout_ puede crear directorios de forma recursiva y poblarlos con los archivos de fuente correspondientes. Puede entonces editar estos archivos fuente en cualquier momento (independientemente de que otros desarrolladores de software están editando sus propias copias de las fuentes); actualizarlos para incluir nuevos cambios aplicados por otros al repositorio de código fuente; o hacer un _commit_ de su trabajo como un cambio permanente en el repositorio de código fuente.

Tenga en cuenta que _checkout_ se utiliza para crear directorios. El directorio de nivel superior creado siempre se añade al directorio donde se invoca _checkout_, y por lo general tiene el mismo nombre que el módulo especificado. En el caso de un alias del módulo, el sub-directorio creado puede tener un nombre diferente, pero usted puede estar seguro de que será un subdirectorio, y que _checkout_ mostrará la ruta relativa que lleva a cada archivo extraído dentro de su área privada de trabajo (a menos que especifique la opción global _"-Q"_).

#####Opciones del comando _checkout_
	-D date

Utiliza la revisión más reciente a más tardar en la fecha. Esta opción es _sticky_, e implica _'-P'_. 

	-f

Sólo útil con las banderas _'-D date'_ o _'-r tag'_. Si no se encuentra la revisión correspondiente, recupera la revisión más reciente (en lugar de ignorar el archivo).

	-k kflag

Procesa las palabras clave de acuerdo con _kflag_. Esta opción es _sticky_; las futuras actualizaciones de este archivo en el directorio de trabajo utilizarán la misma _kflag_. 

	-l

Local; ejecutar sólo en el directorio de trabajo actual.

	-n

No ejecutar cualquier programa _checkout_ (como se especifica con la opción _'-o'_ en el archivo de los módulos).

	-P

Recortar directorios vacíos. 

	-p

Archivos de tubería a la salida estándar.

	-R

Directorios _checkout_ de forma recursiva. Esta opción está activada de forma predeterminada.

	-r tag

Utiliza la etiqueta de revisión. Esta opción es _sticky_, e implica _'-P'_	.

**Además de esas, puede utilizar estas opciones de comandos especiales con _checkout_:**

	-A

Restablece cualquier etiqueta _sticky_, fechas u opciones _'-k'_. No restablece opciones _sticky_ _'-k'_  en los archivos modificados.

	-c

Copia el archivo de módulo, ordenado, a la salida estándar, en lugar de crear o modificar los archivos o directorios en el directorio de trabajo.

	-d dir

Crea un directorio llamado _dir_ para los archivos de trabajo, en lugar de utilizar el nombre del módulo. En general, el uso de esta bandera es equivalente a usar _'mkdir dir; cd dir'_ seguido por el comando _checkout_ sin la etiqueta _' -d '_.

#####Ejemplo del comando _checkout_
Obtiene una copia del módulo _'tc'_:

	$ cvs checkout tc

Obtiene una copia del módulo _'tc'_ como se veía hace un día:

	$ cvs checkout -D yesterday tc


####_commit_ - Revise los archivos en el repositorio

	commit [-lRf] [-m ’log_message’ | -F file] [-r revision] [files…] 

Utilice _commit_ cuando se quiera incorporar los cambios de los archivos fuente de trabajo en el repositorio de origen.

Si no se especifican archivos particulares en _commit_, todos los archivos en el directorio actual de trabajo se examinan. _commit_ es cuidadoso al cambiar en el repositorio sólo aquellos archivos que que realmente han cambiado. Por defecto (o si se especifica explícitamente la opción _'-R'_), se examinan también los archivos de los subdirectorios y se hace _commit_ si han cambiado; puede utilizar la opción _'-l'_ para limitar el _commit_ a sólo el directorio actual.

_commit_ verifica que los archivos seleccionados están al día con las revisiones actuales en el repositorio de origen; se notifica y se sale sin hacer el _commit_, si alguno de los archivos especificados debe actializarse. _commit_ no llama al comando de actualización, sino que deja que se pueda hacer cuando sea el momento adecuado.

Cuando todo está bien, un editor se invoca para permitirle introducir un mensaje de registro que se escribirá a uno o más programas de registro y se coloca en el archivo RCS en el repositorio. Este mensaje de registro se puede recuperar con el comando log. Se puede especificar el mensaje de registro en la línea de comandos con la opción _'-m message'_, y así evitar la invocación editor, o utilizar la opción _"-F file'_ para especificar que el argumento del archivo contiene el mensaje de registro.

#####Opciones del comando _commit_

	-l

Local; ejecuta sólo en el directorio de trabajo actual.

	-R

Directorios _commit_  de forma recursiva. Esto está activada por defecto.

	-r revision

_commit_ a revisión. _revision_ debe ser o bien una rama, o una revisión en el tronco principal que es mayor que cualquier número de revisión existente. No se puede hacer _commit_ a una revisión específica en una rama.

_commit_ también es compatible con las siguientes opciones:

	-F file

Lee el mensaje de registro del archivo, en lugar de invocar un editor.

	-f

Tenga en cuenta que este no es el comportamiento estándar de la opción _'-f'_ como se define en las opciones de comandos comunes.

Fuerza a CVS a hacer _commit_ de una nueva revisión, incluso si no se ha realizado ningún cambio en el archivo. Si la revisión actual del archivo es 1.7, los siguientes dos comandos son equivalentes:
     
	$ cvs commit -f file
	$ cvs commit -r 1.8 file

La opción _'-f'_ deshabilita la recursividad (es decir, implica _'-l'_). Para forzar a CVS para hacer _commit_ de una nueva revisión de todos los archivos en todos los subdirectorios, debe utilizar _'-f -R'_.
	
	-m message

Utiliza _message_ como el mensaje de registro, en lugar de invocar un editor.
