#!/bin/bash

# Actualizar sistema
echo "Actualizando el sistema..."
sudo apt update && sudo apt upgrade -y

# Instalar controladores esenciales
echo "Instalando controladores esenciales..."
sudo apt install -y \
    linux-firmware \
    amd64-microcode \
    firmware-amd-graphics \
    wpasupplicant \
    network-manager \
    openssh-server \
    ufw \
    htop \
    curl \
    wget \
    vim

# Instalar GNOME Core (opcional)
read -p "¿Quieres instalar GNOME Core (interfaz gráfica mínima)? [s/n]: " INSTALL_GNOME
if [[ "$INSTALL_GNOME" == "s" || "$INSTALL_GNOME" == "S" ]]; then
    echo "Instalando GNOME Core..."
    sudo apt install -y gnome-core
    sudo systemctl set-default graphical.target
else
    echo "Omitiendo la instalación de GNOME Core."
fi

# Configurar NetworkManager
echo "Habilitando NetworkManager..."
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager

# Configurar firewall
echo "Configurando firewall (UFW)..."
sudo ufw allow OpenSSH
sudo ufw enable

# Optimización energética
echo "Instalando y configurando powertop..."
sudo apt install -y powertop
sudo powertop --auto-tune

# Limpiar el sistema
echo "Limpiando paquetes innecesarios..."
sudo apt autoremove -y
sudo apt clean

echo "Configuración completada. Reinicia el sistema para aplicar los cambios."
