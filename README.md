# Análisis forense de memoria RAM - A ciegas
Trabajo de análisis forense realizado el 19/11/2024 como caso de estudio académico.
La herramienta usada fue volatility 2.6 en Windows

## Descripción
Un empleado llega a su puesto a las 8:30 y se da cuenta de que ciertos ficheros confidenciales han desaparecido. Esto implicaba que alguien habia accedido sin autorización y robó información sensible. El objetivo del análisis era el de determinar quien accedió al sistema y como por medio de analisis de memoria RAM.

## Objetivos
- Identificar procesos sospechosos
- Detectar conexiones extrañas
- Localizas posible código malicioso
- Encontrar el vector de acceso del atacante
- Aprendizaje y autocrítica

## Metodología
1. Identificación del perfil
   - Win7SP1x64
   - Win2008R2SP0x64
   - Win2008R2SP1x64
   - Win2008R2SP1x64_23418
   - Win7SP1x64_23418
2. Listado de procesos
   - 3 sesiones detectadas:
       - Sesion 0: Sistema
       - Sesión 1: Usuario legítimo
       - Sesión 2: Usuario ilegítimo
   - Existencia de dumpit.exe lo cual indica que alguien llevó a cabo un volcado de memoria
   - Están VBoxService.exe y VBoxTray.exe, lo cual indica el uso de virtualbox
   - Todos los procesos del atacante se ejecutaron en torno a las 7:28, antes que la victima iniciase sesión a las 8:30 
3. Árbol de proceso
   Veo la jerarquia padre-hijo entre procesos
4. Red
   - Conexiones activas entre las 7:28 y 7:30
   - svchost.exe
   - VBoxService.exe aparece poco después reforzando la idea de uso de virtualizaciones por parte del atacante
   - IPs:
       - 10.2.15.137/10.2.15.138
       - 127.0.0.1
5. Detección de código malicioso
- Procesos con indicios de codigo alterado o inyectado:
  - explorer.exe
  - SearchFilterHo
  - dllhost.exe
  - StickyNot.exe 
## Conclusión
- El acceso no autorizado se produjo alrededor de las 7:28
- El atacante accedió desde otro dispositivo
- Se usó virtualbox como posible mecanismo de anonimato
- Parece haber varios procesos que indican inyección de código
- Se ejecutó DumpIt.exe, haciendo posible que el atacante haya realizado su propio volcado de memoria 

[A ciegas -- Volatility](https://github.com/Ch4mbi/A-ciegas--Volatility/blob/main/A%20ciegas-Volatility.md)
