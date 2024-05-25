import java.io.*;
import java.net.*;

public class EnvioArchivo {

    public static void main(String[] args) {
        // Parametros del archivo
        String rutaArchivo = "C:\\Users\\B1SOFT\\Downloads\\Archivo_JUANyJORGE.txt"; // Reemplazar con la ruta del archivo
        long tamanoMaximo = 5 * 1024 * 1024; // 5 MB

        // Parametros del servidor
        String ipServidor = "18.227.102.153"; // Poner la IP del servidor
        int puertoServidor = 3000; // Poner el puerto del servidor

        try {
            // Validar el tamaño del archivo
            File archivo = new File(rutaArchivo);
            if (archivo.length() > tamanoMaximo) {
                System.out.println("Alerta: El archivo debe pesar maximo 5 MB");
                return;
            }

            // Conectar al servidor
            Socket socket = new Socket(ipServidor, puertoServidor);
            System.out.println("Aviso: Conexion establecida exitosamente =)");

            // Enviar el archivo
            OutputStream outputStream = socket.getOutputStream();
            FileInputStream fileInputStream = new FileInputStream(archivo);

            byte[] buffer = new byte[1024];
            int bytesLeidos;
            while ((bytesLeidos = fileInputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, bytesLeidos);
            }

            outputStream.flush();
            fileInputStream.close();
            socket.close();

            System.out.println("Aviso: Archivo enviado exitosamente =)");

        } catch (IOException e) {
            System.out.println("Alerta: No se pudo enviar el archivo :(");
            System.out.println("Motivo: " + e.getMessage());
        }
    }
}
