> Algunos comandos no se admiten si la aplicación usa SQLite como almacén de datos de identidad. Debido a las limitaciones del motor de base de datos, los comandos `Alter` producen la siguiente excepción:
>
> "System. NotSupportedException: SQLite no admite esta operación de migración". 
>
> Como solución alternativa, ejecute Code First migraciones en la base de datos para cambiar las tablas.
