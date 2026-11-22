# 📘 Guia de Adaptação de GSI para Samsung Galaxy A03 Core

> ℹ️ Este guia pode funcionar **não apenas no Samsung Galaxy A03 Core**,  
> mas também em **outros dispositivos Samsung / Samsung Galaxy**,  
> desde que tenham a mesma estrutura de partições (`boot.img`, `vendor.img`, `super.img`)  
> e firmwares no formato AP.

Estas instruções servem para **adaptar uma GSI** (LineageOS / AOSP) caso ocorra um **bootloop** após instalar uma GSI compilada para uma versão de bootloader inferior à do seu aparelho.

---

## ⚠️ Importante

- **Você age por sua própria conta e risco.**  
- O autor não se responsabiliza por brick, perda de dados ou danos ao dispositivo.  
- Este guia destina-se **apenas à adaptação**, quando a versão de firmware usada pelo autor da GSI é inferior à do seu dispositivo.

> Objetivo da adaptação: atualizar o `boot.img` (kernel) e o `super.img` (sistema) para que a GSI funcione corretamente.

---

## 🔹 Passo 1: Identificar a versão de firmware do dispositivo

1. Vá para **Configurações → Sobre o telefone / Sobre o dispositivo**.  
2. Localize **Versão de banda base / Baseband / Modem**.  
3. Essa versão é essencial para escolher os arquivos de adaptação corretos.

> Você precisará de dois arquivos: `boot.img` (kernel) e `super.img` (sistema).

---

## 🔹 Passo 2: Baixar os arquivos necessários

### 2.1 Firmware do dispositivo

- **Para A032F**: [Firmware SM-A032F](https://samfw.com/firmware/SM-A032F)  
- **Para A032M**: [Firmware SM-A032M](https://samfw.com/firmware/SM-A032M)  

Baixe o firmware e extraia a **parte AP** (arquivo que começa com `AP_...`) — ela contém `boot.img` e `super.img`.

> ⚠️ Para a adaptação serão necessárias **duas versões de AP**:
> 1. **AuthorVersion** — versão para a qual o autor compilou a GSI (ou a versão do bootloader que causou o bootloop).  
> 2. **YourVersion** — versão AP instalada atualmente no seu aparelho (deve coincidir com a sua Banda base / Baseband).

Isso é importante porque **o autor da GSI pode não incluir o `boot.img`**, então você precisará extraí-lo do AP correspondente.

### 2.2 Ferramentas

- **MagiskBoot**: [baixar](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/magiskboot)  
- **lpunpack / lpmake** para Linux/Ubuntu (para compilar o `super.img`): [baixar](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/lpunpack_and_lpmake/bin)  
- Para Windows: usamos o `magiskboot.exe`  

---

## 🔹 Passo 3: Preparar a estrutura de pastas

Crie uma pasta principal para a adaptação e duas subpastas dentro dela:

```text
Adaptation/
├─ AuthorVersion/   # versão da GSI que causou o bootloop
└─ YourVersion/     # firmware atual do seu aparelho

```

* Copie o `boot.img` **do AP da versão do autor** (ou do arquivo da GSI, se o autor o incluiu) para a pasta `AuthorVersion/`.
* Copie o seu `boot.img` (extraído do AP do seu firmware) para `YourVersion/`.
* Copie o `magiskboot.exe` para ambas as pastas.

---

## 🔹 Passo 4: Adaptar o kernel (`boot.img`) no Windows

1. Abra o **CMD** (Prompt de Comando) dentro de cada pasta.
2. Descompacte o `boot.img`:

```bash
magiskboot unpack boot.img

```

* Isso extrairá o arquivo **dtb e outros componentes**.

3. Copie o arquivo `dtb` da sua pasta (`YourVersion`) para a pasta do autor (`AuthorVersion`), substituindo o existente.
4. Reempacote o `boot.img`:

```bash
magiskboot repack boot.img

```

* Será gerado o `new-boot.img` — o kernel adaptado.
* Mantenha o `new-boot.img` na mesma pasta onde ficará o `super.img` para o flash.

---

## 🔹 Passo 5: Adaptar o `super.img` (Linux/Ubuntu)

**Recomendação:** use uma máquina virtual com Linux (Ubuntu) para maior praticidade e segurança.

### 5.1: Instalar VirtualBox e Ubuntu

1. Baixe e instale o [VirtualBox](https://www.virtualbox.org/).
2. Instale o Ubuntu no VirtualBox (pode ser a versão LTS mais recente, como 24.04 LTS).

### 5.2: Configurar pastas compartilhadas

Para que a máquina virtual possa acessar os arquivos do seu computador hospedeiro:

1. Instale o **Guest Additions** no Ubuntu:

```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
# depois no VirtualBox: Dispositivos → Inserir imagem de CD dos «Guest Additions»
sudo sh /media/<ponto_montagem_cd>/VBoxLinuxAdditions.run

```

2. No sistema hospedeiro (Windows/Mac/Linux), crie uma pasta de trabalho, por exemplo:

```text
GSI_Adaptation/

```

3. No VirtualBox:

* Selecione sua VM → **Configurações → Pastas Compartilhadas → Adicionar pasta**
* Caminho da pasta no hospedeiro: `GSI_Adaptation/`
* Marque **Montar automaticamente** e **Tornar permanente**

4. No Ubuntu, a pasta estará acessível em um caminho semelhante a:

```bash
/media/sf_GSI_Adaptation/

```

> Nesta pasta copiaremos todas as imagens do AP (`boot.img`, `vendor.img`) e os arquivos extraídos do `super.img`.

### 5.3: Descompactar o `super.img`

1. Descompacte o **`super.img`** da GSI que deu bootloop usando `7zip` ou `lpunpack`.
2. Copie dos arquivos extraídos:

* `system.img`
* `system_ext.ext` (se existir)
* `product.ext` (se existir)

> ⚠️ **Não pegue o `vendor.img` da GSI do autor.** Extraia-o do AP do firmware oficial baixado para o seu aparelho.

3. Coloque todos os arquivos (`system.img`, `system_ext.ext`, `product.ext`, `vendor.img`) na **pasta compartilhada do VirtualBox** (`/media/sf_GSI_Adaptation/` ou pelo hospedeiro).

### 5.4: Compilar o `super.img` via `lpmake`

No terminal do Ubuntu, acesse a pasta compartilhada com os arquivos (`system.img`, `vendor.img`, `system_ext.ext`, `product.ext`) e execute o comando `lpmake` para gerar o novo `super.img`:

```bash
./lpmake \
  --metadata-size 65536 \
  --super-name super \
  --metadata-slots 2 \
  --device super:<device_size> \
  --group main:<group_size> \
  --partition system:readonly:<system_size>:main=system.img \
  --partition vendor:readonly:<vendor_size>:main=vendor.img \
  --partition system_ext:readonly:<system_ext_size>:main=system_ext.ext \
  --partition product:readonly:<product_size>:main=product.ext \
  --output super_new.img

```

#### 🔹 Como descobrir o tamanho dos arquivos para `<system_size>`, `<vendor_size>` e outros

1. Verifique o tamanho exato de cada arquivo em bytes com o comando:

```bash
stat -c%s system.img
stat -c%s vendor.img
stat -c%s system_ext.ext
stat -c%s product.ext

```

> Esses números serão `<system_size>`, `<vendor_size>`, `<system_ext_size>` e `<product_size>`, respectivamente.

2. Caso os tamanhos estejam em megabytes (MB) ou gigabytes (GB), converta-os para bytes:

```text
1 MB = 1024 * 1024 = 1 048 576 bytes
1 GB = 1024 * 1024 * 1024 = 1 073 741 824 bytes

```

**Exemplo:**

* `system.img` = 1.7 GB → 1.7 * 1 073 741 824 ≈ 1 825 360 100 bytes
* `vendor.img` = 0.9 GB → 0.9 * 1 073 741 824 ≈ 966 367 641 bytes

3. Insira os valores obtidos no comando `lpmake`.

#### 🔹 Converter o `super.img` para sparse (opcional)

```bash
./img2simg super_new.img super_new_sparse.img

```

Como resultado, você terá o **`super.img` adaptado**, pronto para ser instalado junto com o `boot.img` adaptado.

💡 **Dica:** utilize a pasta compartilhada da máquina virtual para transferir arquivos facilmente entre o hospedeiro e o Ubuntu sem perder as imagens originais.

---

## 🔹 Passo 6: Preparação para a instalação (Flash)

* Coloque o `new-boot.img` e o `super_new.img` (ou `super_new_sparse.img`) adaptado na mesma pasta.
* Renomeie `new-boot.img` → `boot.img`, e `super_new.img` / `super_new_sparse.img` → `super.img`.
* Empacote os arquivos em um arquivo `.tar` para o **Odin (Windows)** ou instale diretamente pelo **Heimdall (Linux/Mac)**.

---

## 🔹 Passo 7: Instalar a GSI adaptada

* Siga as instruções de [FLASHING_GUIDE_PT](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/FLASHING_GUIDE_PT.md) para **Odin (Windows)** ou **Heimdall (Linux/Mac)**.

---

## ⚠️ Recomendações importantes

* Faça sempre um **backup** antes de instalar qualquer firmware.
* Certifique-se de usar o **firmware correto** correspondente à sua versão de Baseband / Modem.
* Confira tudo **passo a passo** e evite alterar várias partições desnecessárias de uma só vez.
