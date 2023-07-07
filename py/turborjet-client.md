## This is my favorite template:

```PYTHON
from py4j.clientserver import (
    ClientServer,
    PythonParameters,
    JavaParameters
)
from py4j.java_collections import JavaMap
from titan.turbojet.services.executor import executor


class TurboJet(object):
    @staticmethod
    def execute(service: str, params: JavaMap) -> str:
        result = executor(service=service, params=params)
        return result

    class Java:
        implements = ["com.titan.turbojet.TurboJet"]

# Make sure that the Python code is started first.
# Python is server side


if __name__ == "__main__":
    gateway = ClientServer(
    java_parameters=JavaParameters(auto_convert=True, auto_close=True),
    python_parameters=PythonParameters(daemonize=False, daemonize_connections=True),
    python_server_entry_point=TurboJet())
```

## We can use the template with JavaGateway

```PYTHON
from py4j.java_gateway import (
    JavaGateway,
    CallbackServerParameters,
    GatewayParameters
)
from py4j.java_collections import JavaMap
from titan.turbojet.services.executor import executor


class TurboJet(object):
    @staticmethod
    def execute(service: str, params: JavaMap) -> str:
        result = executor(service=service, params=params)
        return result

    class Java:
        implements = ["com.titan.turbojet.TurboJet"]

# Make sure that the Python code is started first.
# Python is server side


if __name__ == "__main__":
    gateway = JavaGateway(
        gateway_parameters=GatewayParameters(auto_convert=True),
        callback_server_parameters=CallbackServerParameters(),
        python_server_entry_point=TurboJet()
    )
```

## In some case, we can customize the template:
```PYTHON
# pylint: disable=C0103,W0212,C0115,R0903
import numpy as np
from py4j.clientserver import ClientServer, JavaParameters
from py4j import protocol as proto
from py4j.java_collections import MapConverter, JavaMap, ListConverter
from titan.turbojet.services.executor import executor


class UserServer(ClientServer):
    def __init__(
        self,
        java_parameters=None,
        python_parameters=None,
        python_server_entry_point=None
    ):
        self.java_parameters = java_parameters
        self.python_parameters = python_parameters
        self.python_server_entry_point = python_server_entry_point
        super().__init__()

    def set_entry_point(self, python_server_entry_point):
        self.gateway_property.pool.put(
            python_server_entry_point,
            proto.ENTRY_POINT_OBJECT_ID
            )


class TurboJet:
    def __init__(self, gateway):
        self.gateway = gateway

    def execute(self, service: str, params: JavaMap) -> JavaMap:
        result = executor(service=service, params=params)
        return self.convert_to_java(result)

    def convert_to_java(self, py_obj: dict) -> JavaMap:
        if isinstance(py_obj, dict):
            jmap = MapConverter().convert(
                {key: self.convert_to_java(value) for key, value in py_obj.items()},
                self.gateway._gateway_client
                )
            return jmap

        if isinstance(py_obj, (list, np.ndarray)):
            java_list = ListConverter().convert(
                [self.convert_to_java(x) for x in py_obj],
                self.gateway._gateway_client
                )
            return java_list

        if isinstance(py_obj, np.generic):
            return py_obj.item()

        return py_obj

    class Java:
        implements = ["com.titan.turbojet.TurboJet"]


# Make sure that the Python code is started first.
if __name__ == "__main__":
    user_gateway = UserServer(java_parameters=JavaParameters(auto_convert=True))
    entry_point = TurboJet(user_gateway)
    user_gateway.set_entry_point(entry_point)

```
