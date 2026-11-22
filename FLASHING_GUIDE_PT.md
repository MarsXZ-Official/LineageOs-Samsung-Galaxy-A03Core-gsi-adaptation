# 📘 GUIA COMPLETO DE INSTALAÇÃO (FLASH) DE GSI VIA ODIN (Windows), ODIN4 (Linux) E HEIMDALL (Mac)

Este guia serve para todos os dispositivos Samsung com bootloader desbloqueado e suporte ao Project Treble (GSI).

---

> ⚠️ **Aviso de isenção de responsabilidade**
> 
> **Você realiza a instalação por sua conta e risco. O autor deste guia não se responsabiliza por quaisquer danos ao aparelho, perda de dados ou mau funcionamento.**

---

## ✅ 1. Requisitos

- Aparelho Samsung com bootloader desbloqueado
- PC com Windows, Linux ou macOS
- Odin (para Windows / Linux) ou Heimdall (para macOS)
- Drivers USB da Samsung (para Windows)
- Cabo USB de boa qualidade
- **Arquivo contendo `super.img`** (e `boot.img`, se houver) — a GSI baixada
- Recomendado: bateria carregada em pelo menos 80%

> ⚠️ **Atenção:** Ao baixar a GSI, você receberá um **arquivo compactado**. **Não é necessário extraí-lo completamente** — o procedimento depende do seu sistema operacional:
> * **No Windows / Linux (Odin / Odin4)**: instale o arquivo `LineageOs20/21.tar` diretamente como está.
> * **No macOS (Heimdall)**: extraia o `LineageOs20/21.tar` para obter `boot.img` (se houver) e `super.img` — são esses arquivos que devem ser instalados.

---

## 🧰 2. Preparação do dispositivo

### 2.1. Ativar as Opções do desenvolvedor

1. Vá para **Configurações → Sobre o telefone**.
2. Toque 7 vezes em **Número de compilação (Build number)**.
3. Volte, entre em **Opções do desenvolvedor** e ative:
   - **Desbloqueio de OEM (OEM Unlocking)**
   - **Depuração USB (USB Debugging)**

### 2.2. Desbloquear o bootloader

> Certifique-se de que o telefone tenha pelo menos 80% de carga.  
> **NÃO conecte o cabo USB agora!** Se estiver conectado, desconecte-o, caso contrário o aparelho entrará no modo de carregamento.

1. Desligue completamente o telefone e aguarde 10 segundos.
2. Pressione e segure simultaneamente **Diminuir volume + Liga/Desliga**.
3. Quando o logotipo da Samsung aparecer, solte o botão **Liga/Desliga**, mas continue segurando **Diminuir volume** até aparecer uma tela azul de aviso.
4. **Conecte o cabo USB** ao PC.
5. Faça um **PRESSIONAMENTO LONGO em Aumentar volume** para entrar no modo de desbloqueio (Device unlock mode).
6. Pressione **Aumentar volume** uma vez para confirmar.
7. O dispositivo apagará todos os dados, desbloqueará o bootloader e reiniciará automaticamente.

### 2.3. Limpeza de dados (Factory Reset)

*(Aguarde o dispositivo inicializar completamente após o desbloqueio e desligue-o novamente).*

1. Entre no modo Recovery (segure **Aumentar volume + Liga/Desliga** ao ligar).
2. Selecione **Wipe data / factory reset**.
3. Selecione **Factory data reset** para confirmar.

### 2.4. Entrar no Download Mode

1. Desligue o dispositivo.
2. Segure **Diminuir volume + Aumentar volume** e **conecte o cabo USB**.
3. Quando aparecer a tela azul de aviso, solte os botões.
4. Dê um **TOQUE CURTO em Aumentar volume** para entrar no Download Mode e iniciar a instalação.

---

## 🪟 3. Instalação via Odin (Windows)

### 3.1. Download e instalação

- [Baixar Odin (recomenda-se 3.14.4)](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/%D0%9Edin)
- [Instalar Samsung USB Drivers](https://developer.samsung.com/android-usb-driver)

### 3.2. Configuração do Odin

1. Abra o Odin.
2. Conecte o aparelho → no log deve aparecer a mensagem **Added!!**
3. Clique no botão **AP** → selecione o **arquivo no formato `.tar`**.
4. Na aba **Options**, verifique se estão marcadas **apenas** as opções:
   - **Auto Reboot**
   - **F. Reset Time**

> ❌ **NÃO marque:** Re-Partition ou Nand Erase.

### 3.3. Iniciar a instalação

Clique em **Start** e aguarde a conclusão (geralmente leva de 1 a 3 minutos).

Se tudo ocorrer bem, o quadrado superior esquerdo ficará verde com a mensagem **PASS!**

### 3.4. Verificar o resultado

O telefone reiniciará automaticamente.

- Se o sistema iniciar — a GSI foi instalada com sucesso.
- Se o aparelho entrar em bootloop (reinicialização infinita) — entre no Recovery e faça um Wipe Data.

---

## 🐧 4. Instalação via Odin4 (Linux)

### 4.1. Download e configuração

1. **Baixe o Odin4:**  
   Acesse a [página do repositório](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/%D0%9Edin/Linux/odin4) e baixe o arquivo `odin4`.
2. **Torne o Odin4 executável:**  
   Altere as permissões do binário pelo terminal:
   ```bash
   chmod +x odin4

```

3. **Mova o `odin4` para o diretório `/bin`:**
Para que o programa fique acessível de qualquer lugar do sistema:
```bash
sudo cp odin4 /bin

```



### 4.2. Processo de instalação

Inicie o flash usando o comando abaixo:

```bash
sudo odin4 -a nome_do_arquivo.tar --reboot

```

**Explicação do comando:**

* `sudo` — execução como administrador (root).
* `odin4` — chamada do próprio programa.
* `-a` — especifica a partição a ser instalada (AP).
* `nome_do_arquivo.tar` — caminho do seu arquivo `.tar`.
* `--reboot` — reinicia automaticamente após a conclusão com sucesso.

---

## 🍏 5. Instalação via Heimdall (macOS)

### 5.1. Instalar o Heimdall

Abra o terminal e instale o Heimdall pelo Homebrew:

```bash
brew install heimdall

```

### 5.2. Preparar os arquivos

1. Baixe o arquivo `.tar` da GSI.
2. Extraia-o no terminal:
```bash
tar -xf nome_do_arquivo.tar

```


3. Você obterá um ou dois arquivos: `boot.img` (se incluído) e `super.img`.
4. Acesse a pasta com os arquivos extraídos:
```bash
cd ~/caminho_dos_arquivos_extraidos

```



### 5.3. Verificar a conexão

Certifique-se de que o telefone esteja em **Download Mode** e conectado ao Mac, depois execute:

```bash
heimdall detect

```

Se o terminal exibir *Device detected*, o aparelho está pronto para o flash.

### 5.4. Instalação

Instale a partição `super`:

```bash
sudo heimdall flash --super super.img

```

> **Observação:** Em alguns dispositivos Samsung, a partição pode se chamar `--super` ou `--SUPER`. Caso ocorra erro, tente alternar entre maiúsculas e minúsculas.

Se o arquivo `.tar` contiver `boot.img`, instale-o também:

```bash
sudo heimdall flash --boot boot.img

```

### 5.5. Conclusão e verificação

O Heimdall mostrará o progresso e reiniciará o aparelho automaticamente.

* O dispositivo iniciou → tudo correu bem.
* O dispositivo entrou em bootloop → entre no Recovery e faça um Wipe Data.
* Erro na instalação → verifique a compatibilidade da GSI baixada com o seu aparelho.

---

## 🎉 Pronto!

**Agora a GSI está instalada e pronta para uso.**
