# CINE-FULL
CINE 
# PROGRAMA: CINE FULL

FILAS = 5
ASIENTOS_POR_FILA = 10

# Funcion para inicializar la sala
def inicializar_sala():
    sala = [[0 for a in range(ASIENTOS_POR_FILA)]
            for f in range(FILAS)]
    return sala


# Funcion para mostrar el estado de la sala
def mostrar_sala(sala):
    print("\n--- ESTADO DE LA SALA ---")
    print("     ", end=" ")

    for a in range(1, ASIENTOS_POR_FILA + 1):
        print(f"{a:3}", end=" ")
    print()

    for f in range(FILAS):
        print(f"F{f+1}:", end=" ")

        for estado in sala[f]:
            if estado == 0:
                simbolo = " D"
            elif estado == 1:
                simbolo = " V"
            else:
                simbolo = " R"

            print(f"{simbolo:3}", end=" ")

        print()

    print("D = Disponible | V = Vendido | R = Reservado")


# Funcion para validar el asiento
def validar_asiento(sala, fila, asiento):
    if (1 <= fila <= FILAS and
        1 <= asiento <= ASIENTOS_POR_FILA):
        return 1
    else:
        return 0


# Funcion para obtener el precio del asiento
def obtener_precio(fila):
    if fila == 1 or fila == 2:
        return 8000
    elif fila == 3 or fila == 4:
        return 6000
    elif fila == 5:
        return 4000
    else:
        return 0


# Funcion para vender un asiento
def vender_asiento(sala, fila, asiento):
    if validar_asiento(sala, fila, asiento) == 0:
        return 0

    indice_fila = fila - 1
    indice_asiento = asiento - 1

    if sala[indice_fila][indice_asiento] == 0:
        sala[indice_fila][indice_asiento] = 1
        return obtener_precio(fila)
    else:
        return 0


# Funcion para devolver un asiento
def devolver_asiento(sala, fila, asiento):
    if validar_asiento(sala, fila, asiento) == 0:
        return 0

    indice_fila = fila - 1
    indice_asiento = asiento - 1

    precio_base = obtener_precio(fila)

    if sala[indice_fila][indice_asiento] == 1:
        sala[indice_fila][indice_asiento] = 2
        penalidad = precio_base * 0.20
        return penalidad

    elif sala[indice_fila][indice_asiento] == 2:
        sala[indice_fila][indice_asiento] = 0
        return -1

    else:
        return 0


# Funcion del menu principal
def menu_principal():
    sala_cine = inicializar_sala()
    ingreso_neto = 0
    total_penalidades = 0
    opcion = 0

    while opcion != 4:
        print("\n==============================")
        print("       MENU: CINE FULL")
        print("==============================")
        print("1. Venta de Asiento")
        print("2. Recoleccion/Devolucion de Asiento")
        print("3. Mostrar Estado de la Sala")
        print("4. Salir")

        try:
            opcion = int(input("¿Cual es su opcion? "))
        except ValueError:
            print("Debe ingresar un numero.")
            continue

        # Opcion 1: Venta de asiento
        if opcion == 1:
            print("\n--- VENTA DE ENTRADAS ---")

            try:
                f = int(input("Ingrese la fila (1-5): "))
                a = int(input("Ingrese el asiento (1-10): "))
            except ValueError:
                print("La fila y el asiento deben ser numeros.")
                continue

            precio_venta = vender_asiento(sala_cine, f, a)

            if precio_venta != 0:
                ingreso_neto += precio_venta
                print(f"Venta exitosa. Precio: ${precio_venta}")
            else:
                print("Error: asiento invalido u ocupado.")

        # Opcion 2: Devolucion de asiento
        elif opcion == 2:
            print("\n--- DEVOLUCION DE ENTRADAS ---")

            try:
                f = int(input("Ingrese la fila (1-5): "))
                a = int(input("Ingrese el asiento (1-10): "))
            except ValueError:
                print("La fila y el asiento deben ser numeros.")
                continue

            resultado = devolver_asiento(sala_cine, f, a)

            if resultado > 0:
                total_penalidades += resultado
                print("Devolucion solicitada.")
                print(f"Penalidad aplicada: ${resultado:.2f}")

            elif resultado == -1:
                precio_base = obtener_precio(f)
                ingreso_neto -= precio_base
                print("Devolucion completada.")
                print(f"Dinero devuelto: ${precio_base}")

            else:
                print("El asiento no se puede devolver.")

        # Opcion 3: Mostrar sala
        elif opcion == 3:
            mostrar_sala(sala_cine)

        # Opcion 4: Salir
        elif opcion == 4:
            print("\n--- RESUMEN DEL DIA ---")
            print(f"Ingreso Total Neto: ${ingreso_neto:.2f}")
            print(f"Total de Penalidades: ${total_penalidades:.2f}")
            print("Gracias por usar Cine Full.")

        else:
            print("Opcion no reconocida.")

# Ejecutar el programa
if _name_ == "_main_":
    menu_principal()
