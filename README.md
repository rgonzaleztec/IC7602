# IC7602
Repositorio para temas del curso de redes

#Código en C# para aplicación en TCP

```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

class TCPClient
{
    static void Main(string[] args)
    {
        try
        {
            // Establecer la dirección IP y el puerto del servidor
            IPAddress ipAddress = IPAddress.Parse("127.0.0.1");
            int port = 8888;

            // Crear una conexión TCP con el servidor
            TcpClient client = new TcpClient();
            client.Connect(ipAddress, port);

            // Obtener la referencia al stream de la red del cliente
            NetworkStream stream = client.GetStream();

            // Mensaje a enviar al servidor
            string message = "Hola, servidor TCP";
            byte[] data = Encoding.ASCII.GetBytes(message);

            // Enviar el mensaje al servidor
            stream.Write(data, 0, data.Length);
            Console.WriteLine("Mensaje enviado al servidor: {0}", message);

            // Buffer para almacenar los datos recibidos
            byte[] buffer = new byte[1024];
            int bytesRead = stream.Read(buffer, 0, buffer.Length);

            // Convertir los datos recibidos en una cadena y mostrarlos en la consola
            string responseData = Encoding.ASCII.GetString(buffer, 0, bytesRead);
            Console.WriteLine("Respuesta del servidor: {0}", responseData);

            // Cerrar la conexión con el servidor
            client.Close();
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error: {0}", ex.Message);
        }
    }
}
```


#Código en C# para aplicación en UDP
```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

class UDPServer
{
    static void Main(string[] args)
    {
        UdpClient server = null;
        try
        {
            // Establecer la dirección IP y el puerto para el servidor
            IPAddress ipAddress = IPAddress.Parse("127.0.0.1");
            int port = 8888;

            // Crear el servidor UDP
            server = new UdpClient(port);
            Console.WriteLine("Servidor UDP iniciado en {0}:{1}", ipAddress, port);

            // Esperar la recepción de datos
            IPEndPoint remoteEP = null;
            byte[] data = server.Receive(ref remoteEP);

            // Convertir los datos recibidos en una cadena y mostrarlos en la consola
            string message = Encoding.ASCII.GetString(data);
            Console.WriteLine("Cliente: {0}", message);

            // Enviar una respuesta al cliente
            string response = "Mensaje recibido por el servidor";
            byte[] responseData = Encoding.ASCII.GetBytes(response);
            server.Send(responseData, responseData.Length, remoteEP);
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error: {0}", ex.Message);
        }
        finally
        {
            // Cerrar el servidor UDP
            server?.Close();
        }
    }
}

```
