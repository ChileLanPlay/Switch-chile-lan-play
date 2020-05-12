# Switch Lan Play Mod

Juega con tus amigos como si fuera Lan

```
                     Internet
                        |
                        |
        IPv4            |          Paquetes LAN
Switch <-------->  PC(Switch LAN Play Mod)  <-------------> Server
                                       UDP
```

# Uso

Para jugar con tus amigos, tu y tus amigos deberan correr Switch LAN Play, conectandose al **mismo** servidor, y establece una IP estática en tu Nintendo Switch.

Tu PC y tu Nintendo Switch **deben** estar conectados al mismo router.

## 1. Windows

1. Instala [WinPcap](https://www.winpcap.org/install/default.htm)

2. Descarga [Switch_Lan_Play_Mod](https://github.com/spacemeowx2/switch-lan-play/releases)

3. Corre Switch_Lan_Play_Mod.exe

4. Ingresa la dirección del servidor (sin puertos). Por ejemplo:

```sh
Switch Lan Play Mod by Macnolo Tech

La direccion del servidor de retransmision es requerida.
Ingresa la direccion del servidor: switch.lan-play.com
```

5. Después te aparecerá esto:

```
1. \Device\NPF_{538AED4A-7BC9-47D9-A1DD-3F8E0AD2D2B0} (Microsoft Corporation)
        IP: [10.0.75.1]
2. \Device\NPF_{A885EB2A-D362-4846-8554-E6F59A044EB9} (Intel(R) Ethernet Connection (2) I219-V)
        IP: [192.168.233.153]
Elige un numero para seleccionar interfaz (1-2):
```

6. Escribe el número de interfaz y estará en modo eschucha

## 2. Switch

1. Ve a Configuración > Internet > Configuración de Internet > {Tu Red} > Modificar, cambia la IP de Automático a Manual. La IP deberá ser cualquiera entre `10.13.0.1` a `10.13.255.254`, exceptuando `10.13.37.1`. Pero no uses la misma IP de tu amigo. Ejemplo:

    <table>
        <tbody>
            <tr>
                <td>Direccion IP</td>
                <td>10.13.0.5</td>
            </tr>
            <tr>
                <td>Mascara de Subred</td>
                <td>255.255.0.0</td>
            </tr>
            <tr>
                <td>Puerta de enlace</td>
                <td>10.13.37.1</td>
            </tr>
        </tbody>
    </table>

2. Guarda y haz una prueba de conexion. Si no conecta, repite la guia.

3. Ejecuta tu juego, mantén L+R+LStick para entrar a Modo LAN. Crea un grupo, diles a tus amigos que se unan y difruta!

# Compilación

## Produce o depura

`cmake -DCMAKE_BUILD_TYPE=Debug ..`
`cmake -DCMAKE_BUILD_TYPE=Release ..`

## Ubuntu / Debian

Instala libpcap0.8-dev y cmake en APT:

`sudo apt install libpcap0.8-dev git gcc g++ cmake`

Ejecuta y compilará:

```sh
mkdir build
cd build
cmake ..
make
```

## Windows

Usa [MSYS2](http://www.msys2.org/) para compilar.

```sh
pacman -Sy
pacman -S make mingw-w64-x86_64-cmake mingw-w64-x86_64-gcc
```

Para compilar en 32 bits:

```sh
pacman -S mingw-w64-i686-cmake mingw-w64-i686-gcc
```

Abre `MSYS2 Mingw`.

```sh
mkdir build
cd build
cmake -G "MSYS Makefiles" ..
make
```

## Mac OS

Instala cmake por Homebrew
```sh
brew install cmake
```
Y luego crea el compilado
```sh
mkdir build
cd build
cmake ..
make
```

# Servidor

## Node JS

```sh
git clone https://github.com/mcn2004/Switch-Lan-Play-Mod
cd Switch-Lan-Play-Mod/server
npm install
npm run build
npm start
```

El servidor estará en escucha en `11451/udp`.

# Protocol

```c
struct packet {
    uint8_t type;
    uint8_t payload[packet_len - 1];
};
```

```c
enum type {
    KEEPALIVE = 0,
    IPV4 = 1,
    PING = 2,
    IPV4_FRAG = 3
};
```