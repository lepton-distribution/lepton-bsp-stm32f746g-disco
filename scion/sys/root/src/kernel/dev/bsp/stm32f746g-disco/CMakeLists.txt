# BSP stm32f746g-disco : paramètres hors manifeste (PA-7 Q3), inclus par le CMakeLists.txt racine
# du greffon noyau quand LEPTON_BOARD=stm32f746g-disco. XML mklepton : LEPTON_MKCONF du preset.
# Puce : FPU simple précision (preset : LEPTON_M7_FPU=sp) ; cœur r0p1, erratum 837070 (bibliothèque
# embOS _837070, USE_ERRATUM_837070=1 pour RTOS.h, manifeste) ; DEV_ID 0x449 et CPUID 0x410FC271
# relus par la sonde (2026-10-05). HSE_VALUE (manifeste) : quartz X2 de la carte (25 MHz).
if(NOT LEPTON_M7_FPU STREQUAL "sp")
  message(FATAL_ERROR "stm32f746g-disco : LEPTON_CPU=cortex-m7 et LEPTON_M7_FPU=sp attendus")
endif()
set(LEPTON_NET_STACK lwip)   # USE_LWIP dans include/user_kernel_mkconf.h
set(LEPTON_EMBOS_LIB_VARIANT _837070)
set(LEPTON_FREERTOS_PORT ARM_CM7/r0p1)   # FreeRTOS : port de l'erratum 837070 (remplace ARM_CM4F du cœur)
set(LEPTON_BOARD_UNAME_MACHINE cortexM7-stm32f7)
# nom vu par uname : posé ici, sans entrée dans la table de kernelconf.h (ajout-coeur.md §4)
target_compile_definitions(lepton_options INTERFACE
  __KERNEL_CPU_DEVICE_NAME="${LEPTON_BOARD_UNAME_MACHINE}")

# carte réelle : console série du ST-LINK (sonde : manifeste, champ openocd)
set(LEPTON_BOARD_SERIAL_PORT "" CACHE STRING
    "Port série de la console de la carte (ex. /dev/ttyACM0) : active le test de fumée sur carte")
# test réseau sur carte (labels board et net, tests/board_net.py) : adresse de la carte (.init),
# fichier téléchargé par FTP et sa source ; adresse de l'hôte sur le câble de la carte : vide, le
# test n'est pas créé
set(LEPTON_NET_TEST_GUEST_IP 192.168.2.5)
set(LEPTON_NET_TEST_FTP_FILE /usr/etc/.boot)
set(LEPTON_NET_TEST_FTP_REFERENCE sys/user/tauon-basic/etc/stm32f746g-disco/.boot)
set(LEPTON_NET_TEST_HOST_IP "" CACHE STRING
    "Adresse de l'hôte sur le lien Ethernet de la carte : active le test réseau sur carte")
