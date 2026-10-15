# Calculadora gRPC

Este proyecto implementa una calculadora básica utilizando **gRPC y Protocol Buffers** para comunicar un cliente y un servidor desarrollados en Python.

## Estructura del proyecto

```text
grpc_calc/
├── proto/
│   └── calculadora.proto
├── generated/
│   ├── calculadora_pb2.py
│   └── calculadora_pb2_grpc.py
├── servidor/
│   └── server.py
├── cliente/
│   └── client.py
├── requirements.txt
└── README.md
```

> **Importante:** Ejecuta los comandos desde la raíz del proyecto (`grpc_calc/`), salvo que se indique lo contrario.

## 1. Crear, activar y desactivar un entorno virtual

Crear el entorno virtual:

```bash
python -m venv env
```

En Linux, si `python` no está disponible, utiliza `python3 -m venv env`.

**Activar en Windows (CMD):**

```cmd
env\Scripts\activate
```

**Activar en Linux:**

```bash
source env/bin/activate
```

Al activarlo aparecerá `(env)` al inicio de la terminal.

**Desactivar (Windows y Linux):**

```bash
deactivate
```

## 2. Instalar dependencias y generar requirements.txt

Con el entorno activado:

```bash
python -m pip install grpcio grpcio-tools protobuf
```

- **grpcio:** biblioteca para ejecutar clientes y servidores gRPC.
- **grpcio-tools:** compilador y herramientas para generar código Python desde `.proto`.
- **protobuf:** serialización y deserialización de mensajes Protocol Buffers.

Guardar las versiones instaladas:

```bash
python -m pip freeze > requirements.txt
```

Para instalar las dependencias en otro entorno, cuando ya existe `requirements.txt`:

```bash
python -m pip install -r requirements.txt
```

## 3. Compilar el archivo .proto en generated/

El contrato del servicio se encuentra en `proto/calculadora.proto`.

Crear la carpeta de salida si no existe:

```bash
mkdir generated
```

Compilar:

```bash
python -m grpc_tools.protoc -I./proto --python_out=./generated --grpc_python_out=./generated ./proto/calculadora.proto
```

Se generan:

- `generated/calculadora_pb2.py`: clases para los mensajes Protobuf.
- `generated/calculadora_pb2_grpc.py`: stubs y clases para implementar el servicio gRPC.

No modifiques manualmente los archivos generados. Si cambias el `.proto`, vuelve a compilarlos.

## 4. Configurar PYTHONPATH

El archivo generado `calculadora_pb2_grpc.py` importa `calculadora_pb2` como módulo de nivel superior. Por ello, agregamos tanto la raíz del proyecto como `generated/` a la ruta de búsqueda de Python.

**Windows (CMD):**

```cmd
set PYTHONPATH=%CD%;%CD%\generated
```

**Linux:**

```bash
export PYTHONPATH="$PWD:$PWD/generated"
```

La configuración es temporal y debe repetirse en cada terminal nueva. Estos comandos asumen que estás situado en la raíz del proyecto.

En `server.py` y `client.py` puedes utilizar:

```python
import generated.calculadora_pb2 as pb2
import generated.calculadora_pb2_grpc as pb2_grpc
```

## 5. Ejecutar el servidor

Con el entorno virtual activado y `PYTHONPATH` configurado:

```bash
python servidor/server.py
```

Deja esta terminal abierta para recibir las solicitudes.

## 6. Ejecutar el cliente

Abre **otra terminal** en la raíz del proyecto. Activa el entorno virtual y configura nuevamente `PYTHONPATH` (pasos 1 y 4).

```bash
python cliente/client.py
```

El cliente se conectará al servidor y podrá invocar las operaciones remotas. Verifica que la dirección y el puerto configurados en `client.py` correspondan al servidor.

## 7. Finalizar

Detén el servidor con **Ctrl + C** y desactiva el entorno virtual:

```bash
deactivate
```

## Consideraciones

- Cuando cambie `calculadora.proto`, vuelve a generar los módulos del paso 3.
- Cuando cambien las dependencias, actualiza `requirements.txt`.
- Para ejecutar cliente y servidor en máquinas diferentes, configura la IP y el puerto de conexión.
