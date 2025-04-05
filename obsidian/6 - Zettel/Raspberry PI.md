
```Bash
ssh finn@raspberrypi-pow
++
```

### Laufwerke
```Bash
lsblk
```

**`NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS`**

Dies ist die Kopfzeile, die die Bedeutung der einzelnen Spalten erklärt:

- **`NAME`**: Der Name des Geräts oder der Partition.
- **`MAJ:MIN`**: Die Major- und Minor-Gerätenummern, die das Gerät im Linux-Kernel identifizieren.
- **`RM`**: Gibt an, ob es sich um ein Wechselmedium handelt (`1` für ja, `0` für nein).
- **`SIZE`**: Die Größe des Geräts oder der Partition.
- **`RO`**: Gibt an, ob das Gerät schreibgeschützt ist (`0` für nein, `1` für ja).
- **`TYPE`**: Der Typ des Geräts (`disk` für eine gesamte Festplatte/USB-Stick, `part` für eine Partition).
- **`MOUNTPOINTS`**: Der Ort im Dateisystem, wo die Partition eingehängt ist (falls zutreffend).

#### Formatieren

```Bash
# Partitionen anzeigen
sudo fdisk -l /dev/sda
# Start von fdisk
sudo fdisk /dev/sda
```

![[Pasted image 20250328143436.png]]

#### Mounten

```Bash
# Ordner erstellen
mkdir ordner
# Ordner zur Partition mounten
sudo mount <partition> <ordner>
# Vor dem physischen Entfernen unmounten
sudo unmount  <partition> <ordner>
```

### Hallo