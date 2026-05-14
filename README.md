# Infraestrutura de VPN Anti-Censura

## Sobre o Projeto
Este repositório contém a infraestrutura em Docker para implantar uma rede privada blindada contra inspeção profunda de pacotes (DPI), essa rede permite que o inbound abra um tunel https atraves de um certificado confiavel de grandes corporações, esse tunel é criptografado e permite direcionamento de trafego entre o cliente e o servidor, esse servidor passa a se comunicar externamente com a internet, esse método permite o encapsulamento de diferentes protolos proibidos pelo DPI. O roteamento é feito por meio de um painel 3X-UI (Xray-core) para a ofuscação de tráfego.

### Utilidades e Casos de Uso
* **Evasão de Censura e Firewalls:** Fura bloqueios de redes restritas (corporativas ou acadêmicas), mascarando o tráfego do usuário como navegação HTTPS comum.
* **Acesso Remoto Seguro (Bypass de CGNAT):** Permite acesso SSH limpo e criação de túneis reversos para expor aplicações locais.
* **Jogos e Projetos em Rede:** Estabelece rotas de baixa latência para partidas cooperativas (ex: Helldivers 2, gerenciamento de servidores modificados de Minecraft) sem sofrer com interferências da operadora.
* **Intranet:** Permite roteamento de serviços que não existe na internet pública e só pode ser acessado por usuários ativamente conectados ao túnel ofuscado.

## Tipos de Setup (Onde Instalar)
Essa arquitetura foi desenhada para rodar de forma contêinerizada e agnóstica através do **Docker**.
* **Produção (Recomendado):** Uma VPS (Virtual Private Server) dedicada rodando distribuições leves como Debian ou Ubuntu Server.
* **Homelab / Desenvolvimento:** Pode ser executado em máquinas locais, desde que a máquina possua Docker, o firewall seja configurado para roteamento e a rede possua IP público com as portas devidamente expostas.

## Instruções de Execução

### Pré-requisitos
* Docker e plugin Docker Compose instalados no servidor.

### Passos para Inicialização
1. Clone este repositório no servidor e acesse ele:
    ```bash
    git clone url-do-repositorio
    cd evasao-de-DPI-em-redes-corporativas
    ```
2. Suba a infraestrutura:
    ```bash
    docker compose up
    ```
3. Acesse o painel pela url padrão do painel:
    ```bash
    http://[localhost ou ip da VPS]:2053
    ```
    Credenciais padrão para o primeiro acesso:
    * Usuário: admin
    * Senha: admin
4. Desligar a rede:
    ```bash
    docker compose down
    ```

## Configuração do Túnel (Modelo de Inbound)
Após subir o painel 3X-UI e acessá-lo via navegador, crie o ponto de entrada (Inbound) com os parâmetros abaixo para garantir a furtividade máxima da conexão:

| Campo | Configuração Obrigatória |
| :--- | :--- |
| **Protocolo** | `vless` |
| **Porta** | `443` |
| **Security** | `reality` |
| **uTLS** | `chrome` |
| **Dest** / **SNI** | `www.microsoft.com:443` / `www.microsoft.com` |

## 💻 Clientes Necessários (Para Usuários)
Para se conectar ao túnel, os usuários precisam instalar um cliente compatível com o protocolo VLESS + REALITY.

* **Nekoray (Recomendado):** Ideal para sistemas Windows e Linux. Possui o **Modo TUN** nativo, que captura 100% do tráfego do sistema operativo, incluindo terminais (SSH), navegadores e jogos.
* **v2rayN:** Alternativa clássica e muito leve para Windows.
* **v2rayNG:** O cliente padrão para dispositivos Android.
* **FoXray / V2Ray Tun:** Excelentes opções para usuários do ecossistema iOS/Apple.

*Instrução ao usuário: Após importar o link gerado pelo servidor no seu cliente, lembre-se de ativar o Modo TUN (ou System Proxy) e iniciar a conexão para garantir que todo o tráfego da máquina seja protegido pelo túnel.*