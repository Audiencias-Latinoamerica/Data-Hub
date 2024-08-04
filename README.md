<p align="center">
<image
  src="https://github.com/Audiencias-Latinoamerica/.github/assets/4085605/ff9828b4-a69f-4c68-9f2c-7ba2c7a354ab"
  height=200
  margin=0>
</p>



# Data Hub

## Metodologías y Métricas

Realizamos métricas de audiencias utilizando las siguientes metodologías: Rating, Share, Reach, y ATS. Estas métricas se procesan a partir de datos proporcionados por nuestros socios y se calculan en intervalos de Minuto a Minuto, 30 minutos y 60 minutos.

### Métricas

- **SHARE:** 
  ```
  SHARE = (Cantidad de usuarios únicos activos en el canal en el slot de tiempo seleccionado / La cantidad de usuarios únicos activos en el slot de tiempo seleccionado) * 100
  ```

- **RATING/REACH:** 
  ```
  RATING/REACH = (Cantidad de usuarios únicos activos en el canal en el slot de tiempo seleccionado / La cantidad de usuarios únicos activos en el día encuestado)
  ```
  >El rating en 30 y 60 minutos es el promedio generado en ese slot de tiempo.

- **ATS:**
  ```
  ATS = (Cantidad de usuarios activos en el canal en el slot de tiempo seleccionado / La cantidad de usuarios únicos activos en el canal y el slot de tiempo seleccionado)
  ```

## Carga de datos por parte de los socios

### Instructions for Uploading Data to S3

Para facilitar la transferencia de datos, hemos configurado un bucket S3 donde puedes depositar tus datos en las carpetas correspondientes. El nivel de seguridad de los datos es alto ya que los buckets son seguros. Para acceder al bucket, necesitarás una clave de acceso y una clave secreta de AWS. A continuación, se presentan las instrucciones y las categorías de datos para la carga.

### Folder Structure

- **Activity**
  - **Descripción:** Datos relacionados con la actividad de set-top boxes u OTTs.
  - **Ruta:** `s3://<bucket-name>/activity/`
  - **Formatos de archivo recomendados:** Parquet, JSONL, CSV, TSV

- **Users**
  - **Descripción:** Información de usuarios con datos sensibles anonimizados utilizando SHA256 y MD5.
  - **Ruta:** `s3://<bucket-name>/users/`
  - **Formatos de archivo recomendados:** Parquet, JSONL, CSV, TSV
  - **Nota:** Asegúrate de que toda la información sensible esté correctamente anonimizada.

- **EPG**
  - **Descripción:** Guía Electrónica de Programas (EPG) e información de canales.
  - **Ruta:** `s3://<bucket-name>/epg/`
  - **Formatos de archivo recomendados:** Parquet, JSONL, CSV, TSV

- **Devices**
  - **Descripción:** Información sobre los dispositivos utilizados.
  - **Ruta:** `s3://<bucket-name>/devices/`
  - **Formatos de archivo recomendados:** Parquet, JSONL, CSV, TSV

### General Recommendations

- **Formatos de archivo:** Recomendamos utilizar formatos estándar como Parquet, JSONL, CSV o TSV para facilitar la integración y el análisis de datos.
- **Nomenclatura de archivos:** Usa nombres descriptivos y fechas en el formato `YYYYMMDD` para facilitar la identificación. Ejemplo: `activity_20240709.parquet`
- **Carga de archivos:**
  - Utiliza AWS CLI o cualquier herramienta compatible con S3 para cargar archivos.
  - Asegúrate de tener los permisos necesarios para escribir en las carpetas correspondientes.
  - Verifica que los archivos se hayan cargado correctamente.

### Installing AWS CLI

Para cargar archivos utilizando AWS CLI, primero debes instalarlo:

- **Windows:**
  - Descarga el instalador desde la [Guía de Instalación de AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html).
  - Ejecuta el instalador y sigue las instrucciones.

- **macOS:**
  - Utiliza Homebrew para instalar AWS CLI:
    ```bash
    brew install awscli
    ```

- **Linux:**
  - Utiliza los siguientes comandos para instalar AWS CLI:
    ```bash
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
    ```

Después de la instalación, configura AWS CLI con tus credenciales:

```bash
aws configure
```

Ingresa tu AWS Access Key ID, Secret Access Key, región y formato de salida cuando se te solicite.

### Using Boto3 for Uploading Files

Como alternativa, puedes usar la biblioteca Boto3 en Python para cargar archivos a S3. A continuación se muestra un script de ejemplo:

- **Instalar Boto3:**
  ```bash
  pip install boto3
  ```

- **Script de ejemplo en Python:**
  ```python
  import boto3
  from botocore.exceptions import NoCredentialsError

  def upload_to_aws(local_file, bucket, s3_file):
      s3 = boto3.client('s3')
      try:
          s3.upload_file(local_file, bucket, s3_file)
          print("Upload Successful")
          return True
      except FileNotFoundError:
          print("The file was not found")
          return False
      except NoCredentialsError:
          print("Credentials not available")
          return False

  uploaded = upload_to_aws('local_file.parquet', '<bucket-name>', 'activity/local_file.parquet')
  ```

### AWS CLI Command Examples

Para aquellos que prefieren usar la línea de comandos, aquí hay algunos ejemplos de cómo cargar archivos:

- **Cargar un archivo en Activity:**
  ```bash
  aws s3 cp activity_20240709.parquet s3://<bucket-name>/activity/ --recursive
  ```

- **Cargar un archivo en Users:**
  ```bash
  aws s3 cp users_20240709.jsonl s3://<bucket-name>/users/ --recursive
  ```

- **Cargar un archivo en EPG:**
  ```bash
  aws s3 cp epg_20240709.csv s3://<bucket-name>/epg/ --recursive
  ```

- **Cargar un archivo en Devices:**
  ```bash
  aws s3 cp devices_20240709.tsv s3://<bucket-name>/devices/ --recursive
  ```

## Proceso de Consumo

Los socios pueden acceder a sus datos procesados de dos maneras:

1. **Informe de Looker:** Acceso a informes detallados y visualizaciones de datos.
2. **Depósito S3:** Descarga de datos procesados en formato parquet desde un depósito S3.

## Tablas 
>En construcción
- `minute_sequence`
- `sixty-minute-channel`
- `thirty-minute-channel`
- `minute-by-minute-channel`

## Información de Contacto

Para cualquier consulta o soporte, puedes contactarnos a través de los siguientes correos electrónicos:

- **Consultas generales:** [jm@bb-media.com](mailto:jm@bb-media.com) / [nw@bb-media.com](mailto:nw@bb-media.com)
- **Soporte técnico:** [support@bb-media.com](mailto:support@bb-media.com)
