import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class BancoApp {

    private static final Map<String, Double> tasasDeInteres = new HashMap<>();

    static {
        tasasDeInteres.put("Ahorro", 2.5);
        tasasDeInteres.put("Corriente", 0.5);
    }

    private static final Object lock = new Object();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Bienvenido al Banco Online!");
        System.out.println("Seleccione una opción:");
        System.out.println("1. Crear nueva cuenta");
        System.out.println("2. Consultar tasas de interés");

        int opcion = scanner.nextInt();
        scanner.nextLine();  // limpiar buffer

        switch (opcion) {
            case 1:
                crearCuenta(scanner);
                break;
            case 2:
                mostrarTasas();
                break;
            default:
                System.out.println("Opción no válida.");
        }
        scanner.close();
    }

    private static void crearCuenta(Scanner scanner) {
        synchronized (lock) {
            System.out.println("Ingrese su nombre:");
            String nombre = scanner.nextLine();
            System.out.println("Ingrese tipo de cuenta (Ahorro/Corriente):");
            String tipoCuenta = scanner.nextLine();

            if (!tasasDeInteres.containsKey(tipoCuenta)) {
                System.out.println("Tipo de cuenta inválido.");
                return;
            }

            System.out.println("Creando cuenta para " + nombre + " tipo " + tipoCuenta + "...");
            try {
                Thread.sleep(1000); // simulando latencia
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }

            System.out.println("Cuenta creada exitosamente!");
        }
    }

    private static void mostrarTasas() {
        System.out.println("Tasas de Interés:");
        tasasDeInteres.forEach((tipo, tasa) ->
            System.out.println(tipo + ": " + tasa + "% anual")
        );
    }
}
