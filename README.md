// Juan y Jorge Ireta
//Envio de archivo a servidor

import java.io.*;
import java.net.*;

public class EnvioArchivo {

    public static void main(String[] args) {
        // Parámetros del archivo
        String rutaArchivo = "C:\\Users\\B1SOFT\\Downloads\\Archivo_JUANyJORGE.txt"; // Reemplazar con la ruta del archivo
        long tamanoMaximo = 5 * 1024 * 1024; // Este es el tamaño maximo permitido del archivo 5 MB

        // Parámetros del servidor
        String ipServidor = "18.227.102.153"; // Aqui pongo la IP del servidor
        int puertoServidor = 3000; // Aqui definimos el puerto del servidor

        try {
            // Validamos el tamaño del archivo
            File archivo = new File(rutaArchivo);
            if (!archivo.exists()) {
                throw new FileNotFoundException("Alerta: El archivo no se encontró: " + rutaArchivo);
            }
            if (archivo.length() > tamanoMaximo) {
                throw new IllegalArgumentException("Alerta: El archivo debe pesar máximo 5 MB");
            }

            // Conectar al servidor
            Socket socket = new Socket(ipServidor, puertoServidor);
            System.out.println("Aviso: Conexión establecida exitosamente");

            // Enviar el archivo
            try (OutputStream outputStream = socket.getOutputStream();
                 FileInputStream fileInputStream = new FileInputStream(archivo)) {

                byte[] buffer = new byte[1024];
                int bytesLeidos;
                while ((bytesLeidos = fileInputStream.read(buffer)) != -1) {
                    try {
                        outputStream.write(buffer, 0, bytesLeidos);
                    } catch (IOException e) {
                        System.err.println("Alerta: Error al escribir en el socket: " + e.getMessage());
                        throw e; // Propagar la excepción para cerrar el socket
                    }
                }
                outputStream.flush();

                System.out.println("Aviso: Archivo enviado exitosamente");

            } catch (IOException e) {
                System.err.println("Alerta: Error al enviar el archivo: " + e.getMessage());
            } finally {
                try {
                    socket.close();
                } catch (IOException e) {
                    System.err.println("Alerta: Error al cerrar el socket: " + e.getMessage());
                }
            }

        } catch (FileNotFoundException e) {
            System.err.println(e.getMessage());
        } catch (IllegalArgumentException e) {
            System.err.println(e.getMessage());
        } catch (UnknownHostException e) {
            System.err.println("Alerta: No es posible establecer conexión con el servidor: " + e.getMessage());
        } catch (IOException e) {
            System.err.println("Alerta: Error al conectar al servidor: " + e.getMessage());
        }
    }
}
