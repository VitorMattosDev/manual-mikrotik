# ROADMAP — Manual de MikroTik

Plano completo da obra, capítulo a capítulo, em ordem de execução. Este
arquivo é a **fila de trabalho autoritativa**: define o que criar, em que
volume, com que caminho de arquivo, com que pré-requisitos e em que ordem.

## Protocolo de execução (para o agente)

A próxima tarefa é **sempre o primeiro item não marcado `[ ]`** na ordem em
que aparece abaixo. Para executá-la:

1. Use exatamente o **caminho de arquivo** indicado no item.
2. Escreva o capítulo seguindo as convenções de `CLAUDE.md` e espelhando o
   gabarito `volumes/v01-primeiros-passos/01-o-que-e-mikrotik.qmd`
   (objetivos → corpo didático → 🧪 Laboratório → resumo → bibliografia).
3. Garanta que o arquivo está registrado em `chapters:` no `_quarto.yml`. Ao
   **iniciar um volume novo**, descomente (ou crie) o bloco `part:` dele.
4. Valide com `quarto render` (lembre o bootstrap §0 do CLAUDE.md).
5. Marque o item como `[x]` aqui e faça um commit (`cap NN: <título>`).

**Regra de ritmo:** um capítulo por vez, um por sessão. Fatia vertical: não
comece o próximo volume antes de fechar o anterior; não comece a próxima fase
antes de fechar a anterior. Não pule itens.

Legenda: `[x]` escrito · `[~]` esboço/stub a completar · `[ ]` pendente.

---

# FASE 1 — Fundamentos e operação básica

## Volume 1 — Primeiros passos
*Objetivo: entender o que é o MikroTik/RouterOS, acessar o equipamento com
segurança e montar o laboratório CHR usado no livro inteiro.
Pré-requisitos: nenhum.*

- [x] `volumes/v01-primeiros-passos/01-o-que-e-mikrotik.qmd` — O que é MikroTik e RouterOS: história, RouterOS vs. SwOS, onde o MikroTik se encaixa na rede de um provedor fibra+rádio. **(gabarito de estilo)**
- [x] `volumes/v01-primeiros-passos/02-hardware-linhas-produto.qmd` — Linhas de hardware: hEX, hAP, CRS, CCR, LHG/SXT; como escolher equipamento por função (borda, concentrador, switch, rádio). *Pré-req.: cap. 1.*
- [x] `volumes/v01-primeiros-passos/03-licencas-routerboot.qmd` — Licenças RouterOS (níveis 0–6, licença do CHR) e RouterBOOT (bootloader, proteções). *Pré-req.: cap. 2.*
- [x] `volumes/v01-primeiros-passos/04-formas-de-acesso.qmd` — Formas de acesso: WinBox, WebFig, SSH, serial, MAC-Telnet; quando usar cada uma. *Pré-req.: cap. 1.*
- [x] `volumes/v01-primeiros-passos/05-laboratorio-chr.qmd` — **Capítulo-chave**: montando o laboratório com CHR no VirtualBox/GNS3/EVE-NG; topologia-base reutilizada em todo o livro. Recebe a âncora `{#sec-lab-chr}`. *Pré-req.: cap. 4.*
- [x] `volumes/v01-primeiros-passos/06-primeiro-acesso-seguranca.qmd` — Primeiro acesso, configuração padrão, usuários, grupos, senhas e segurança inicial (desabilitar serviços, allowed-address). *Pré-req.: cap. 5.*
- [x] `volumes/v01-primeiros-passos/07-backup-export-reset.qmd` — Backup binário vs. export, restauração, reset de configuração (com avisos). *Pré-req.: cap. 6.*
- [~] `volumes/v01-primeiros-passos/08-netinstall.qmd` — Netinstall: reinstalação e recuperação de equipamentos. *Pré-req.: cap. 7.*

## Volume 2 — CLI e configuração essencial
*Objetivo: dominar o terminal e construir um roteador de borda funcional.
Pré-requisitos: Vol. 1.*

- [x] `volumes/v02-cli-configuracao/01-anatomia-terminal.qmd` — Anatomia do terminal: menus, `print`, `add`, `set`, `remove`, numeração de itens, TAB, `?`, `export` de seções.
- [x] `volumes/v02-cli-configuracao/02-enderecamento-ip-rota-padrao.qmd` — Endereçamento IP no RouterOS e rota padrão (gateway, distância, check-gateway básico).
- [x] `volumes/v02-cli-configuracao/03-dhcp.qmd` — DHCP client (WAN) e DHCP server (LAN): pools, leases, opções.
- [x] `volumes/v02-cli-configuracao/04-dns.qmd` — DNS: resolver, cache, allow-remote-requests e riscos, entradas estáticas.
- [x] `volumes/v02-cli-configuracao/05-nat-masquerade.qmd` — NAT masquerade: por que a LAN privada precisa dele; srcnat vs. masquerade.
- [x] `volumes/v02-cli-configuracao/06-projeto-roteador-de-borda.qmd` — **Projeto integrador**: roteador de borda completo do zero (WAN DHCP + LAN + DHCP server + DNS + NAT).

## Volume 3 — Firewall
*Objetivo: entender e operar o firewall do RouterOS com segurança de provedor.
Pré-requisitos: Vol. 2.*

- [x] `volumes/v03-firewall/01-connection-tracking.qmd` — Connection tracking: estados (new/established/related/invalid), tabela de conexões.
- [x] `volumes/v03-firewall/02-chains-logica-firewall.qmd` — Chains input/forward/output e a lógica de avaliação das regras (ordem, if-first-match).
- [x] `volumes/v03-firewall/03-protegendo-o-roteador.qmd` — Filter na chain input: protegendo o próprio roteador (serviços, ICMP, drop final).
- [x] `volumes/v03-firewall/04-forward-address-lists.qmd` — Filter na chain forward e address-lists (estáticas e dinâmicas, timeout).
- [x] `volumes/v03-firewall/05-dstnat-port-forwarding.qmd` — dstnat e port forwarding: publicando serviços, hairpin NAT.
- [x] `volumes/v03-firewall/06-mangle-raw.qmd` — Mangle (marcação de pacote/conexão/rota) e RAW (notrack, drop antes do conntrack).
- [x] `volumes/v03-firewall/07-hardening-completo.qmd` — Hardening completo do roteador de provedor: checklist consolidado.

## Volume 4 — Bridge, switching e VLANs
*Objetivo: segmentar a rede em camada 2 com desempenho de hardware.
Pré-requisitos: Vol. 2 (Vol. 3 recomendado).*

- [x] `volumes/v04-bridge-vlans/01-bridge-hardware-offload.qmd` — Bridge: conceito, portas, hardware offload e quando ele se perde.
- [x] `volumes/v04-bridge-vlans/02-vlans-8021q.qmd` — VLANs e 802.1Q: tagging, interface vlan sobre bridge/ethernet.
- [x] `volumes/v04-bridge-vlans/03-bridge-vlan-filtering.qmd` — Bridge VLAN filtering: tagged/untagged, PVID, trunk e access na prática.
- [x] `volumes/v04-bridge-vlans/04-switch-chip-crs.qmd` — Switch chip nos CRS: switching em hardware, diferenças CRS1xx/3xx.
- [ ] `volumes/v04-bridge-vlans/05-segmentacao-rede-provedor.qmd` — Cenários de segmentação: gerência, clientes, serviços; projeto de VLANs de um POP.

---

# FASE 2 — Provedor na prática

## Volume 5 — Wireless e enlaces de rádio
*Objetivo: construir e manter enlaces PtP/PtMP rurais e WiFi de cliente.
Pré-requisitos: Vols. 3 e 4.*

- [ ] `volumes/v05-wireless/01-fundamentos-radio-regulamentacao.qmd` — Bandas (2.4/5/60 GHz), regulamentação Anatel, potência, antenas e visada.
- [ ] `volumes/v05-wireless/02-wireless-routeros.qmd` — Wireless no RouterOS: modos (ap-bridge, station, bridge), registration-table, segurança.
- [ ] `volumes/v05-wireless/03-ptp-nstreme-nv2.qmd` — Enlaces PtP com protocolos proprietários: Nstreme e NV2 (TDMA); quando usar cada um.
- [ ] `volumes/v05-wireless/04-ptmp-rural.qmd` — PtMP rural: setorial + LHG/SXT nos clientes, access-list, planejamento de célula.
- [ ] `volumes/v05-wireless/05-alinhamento-troubleshooting.qmd` — Alinhamento (signal, CCQ, SNR) e troubleshooting de enlace degradado.
- [ ] `volumes/v05-wireless/06-capsman-wifi-moderno.qmd` — CAPsMAN e WiFi wave2/ax (pacote wifi do v7) para a casa do cliente.

## Volume 6 — PPPoE e autenticação de assinantes
*Objetivo: autenticar, endereçar e gerenciar assinantes como um ISP real.
Pré-requisitos: Vols. 3 e 4.*

- [ ] `volumes/v06-pppoe/01-como-funciona-pppoe.qmd` — Como funciona o PPPoE: descoberta, sessão, MTU/MRU e por que ISPs o usam.
- [ ] `volumes/v06-pppoe/02-pppoe-server-profiles-pools.qmd` — PPPoE server: interface, profiles, pools, secrets locais; PPPoE client para teste.
- [ ] `volumes/v06-pppoe/03-radius-user-manager-gestao.qmd` — RADIUS e User Manager; integração com sistemas de gestão de provedor (atributos, CoA/desconexão).
- [ ] `volumes/v06-pppoe/04-boas-praticas-concentrador.qmd` — Boas práticas de concentrador (BNG): dimensionamento, keepalive, segurança, múltiplos concentradores.

## Volume 7 — QoS e controle de banda
*Objetivo: entregar planos de velocidade justos e previsíveis.
Pré-requisitos: Vols. 3 e 6.*

- [ ] `volumes/v07-qos/01-fundamentos-qos-simple-queues.qmd` — Fundamentos de filas e simple queues: max-limit, target, ordem das filas.
- [ ] `volumes/v07-qos/02-burst-planos-velocidade.qmd` — Burst (limit-at, burst-limit/threshold/time) e desenho de planos de velocidade.
- [ ] `volumes/v07-qos/03-mangle-queue-tree.qmd` — Marcação com mangle e queue tree: priorização por tipo de tráfego.
- [ ] `volumes/v07-qos/04-pcq-em-escala.qmd` — PCQ: controle de banda por assinante em escala; PCQ + PPPoE.

## Volume 8 — Roteamento
*Objetivo: rotear a rede do provedor com OSPF e BGP no RouterOS v7.
Pré-requisitos: Vol. 3.*

- [ ] `volumes/v08-roteamento/01-rotas-estaticas-distancia.qmd` — Rotas estáticas, distância administrativa, check-gateway, blackhole.
- [ ] `volumes/v08-roteamento/02-ospf-conceitos-configuracao.qmd` — OSPF: conceitos (LSA, DR/BDR, custo) e configuração no v7 (instance/area/template).
- [ ] `volumes/v08-roteamento/03-ospf-avancado.qmd` — OSPF avançado: múltiplas áreas, autenticação, custos, filtragem, stub.
- [ ] `volumes/v08-roteamento/04-bgp-conceitos-peering.qmd` — BGP: AS, eBGP/iBGP, sessão com operadora/IX, anúncio de prefixos.
- [ ] `volumes/v08-roteamento/05-bgp-filtros-communities.qmd` — BGP: routing filters do v7, communities, prepend, boas práticas de segurança (RPKI conceitual).
- [ ] `volumes/v08-roteamento/06-vrf-boas-praticas-v7.qmd` — VRF e boas práticas de roteamento no RouterOS v7 (tabelas, rules).

## Volume 9 — CGNAT e IPv6
*Objetivo: sobreviver ao esgotamento IPv4 e implantar IPv6 de verdade.
Pré-requisitos: Vols. 6 e 8.*

- [ ] `volumes/v09-cgnat-ipv6/01-cgnat-netmap.qmd` — CGNAT: conceito, 100.64.0.0/10, netmap determinístico, portas por assinante.
- [ ] `volumes/v09-cgnat-ipv6/02-logging-requisitos-legais.qmd` — Logging de CGNAT e requisitos legais brasileiros (Marco Civil): o que guardar e como.
- [ ] `volumes/v09-cgnat-ipv6/03-ipv6-fundamentos-routeros.qmd` — IPv6 no RouterOS: endereçamento, ND, SLAAC, pools.
- [ ] `volumes/v09-cgnat-ipv6/04-dhcpv6-pd-dual-stack-pppoe.qmd` — DHCPv6-PD e dual stack no PPPoE: delegando /56 ao assinante.
- [ ] `volumes/v09-cgnat-ipv6/05-firewall-ipv6.qmd` — Firewall IPv6: diferenças do v4, ICMPv6, proteção da LAN do cliente.

---

# FASE 3 — Avançado e operação

## Volume 10 — Túneis e VPN
*Objetivo: interligar POPs e dar acesso remoto seguro.
Pré-requisitos: Vols. 3 e 8.*

- [ ] `volumes/v10-tuneis-vpn/01-panorama-tuneis.qmd` — Panorama de túneis no RouterOS: quando usar cada um; overhead e MTU.
- [ ] `volumes/v10-tuneis-vpn/02-wireguard.qmd` — WireGuard: chaves, peers, roteamento; acesso de gerência e site-to-site.
- [ ] `volumes/v10-tuneis-vpn/03-ipsec.qmd` — IPsec: IKEv2, policies vs. tunnel mode, interoperabilidade.
- [ ] `volumes/v10-tuneis-vpn/04-gre-eoip-l2tp-sstp-zerotier.qmd` — GRE, EoIP, L2TP/SSTP e ZeroTier: camada 2 sobre camada 3 e acessos legados.
- [ ] `volumes/v10-tuneis-vpn/05-interligacao-pops.qmd` — Casos de provedor: interligação de POPs por túnel (com e sem IP público).

## Volume 11 — MPLS
*Objetivo: backbone de provedor com MPLS.
Pré-requisitos: Vols. 8 e 10.*

- [ ] `volumes/v11-mpls/01-conceitos-ldp.qmd` — MPLS: labels, LSP, por que provedores usam; LDP no RouterOS.
- [ ] `volumes/v11-mpls/02-vpls.qmd` — VPLS: camada 2 fim a fim sobre o backbone (transporte de clientes dedicados).
- [ ] `volumes/v11-mpls/03-traffic-engineering.qmd` — Traffic Engineering: túneis TE, reserva de banda, caminhos explícitos.

## Volume 12 — Scripts e automação
*Objetivo: automatizar operação e provisionamento.
Pré-requisitos: Vol. 2 (Vol. 3 recomendado).*

- [ ] `volumes/v12-scripts/01-linguagem-script.qmd` — Linguagem de script do RouterOS: variáveis, condicionais, loops, funções, parsing.
- [ ] `volumes/v12-scripts/02-scheduler-netwatch-backups.qmd` — Scheduler, netwatch e backups automáticos (e-mail/FTP/fetch).
- [ ] `volumes/v12-scripts/03-api-provisionamento.qmd` — API (binária e REST) e provisionamento em massa de equipamentos.

## Volume 13 — Monitoramento e diagnóstico
*Objetivo: enxergar a rede e diagnosticar com método.
Pré-requisitos: Vol. 3.*

- [ ] `volumes/v13-monitoramento/01-logs-syslog.qmd` — Logs: topics, actions, syslog remoto centralizado.
- [ ] `volumes/v13-monitoramento/02-snmp-graphing-dude.qmd` — SNMP, graphing e The Dude: monitorando o parque do provedor.
- [ ] `volumes/v13-monitoramento/03-torch-sniffer-profile.qmd` — Torch, packet sniffer e profile: enxergando tráfego e CPU em tempo real.
- [ ] `volumes/v13-monitoramento/04-metodologia-troubleshooting.qmd` — Metodologia de troubleshooting: da reclamação do cliente à causa raiz.

## Volume 14 — Alta disponibilidade
*Objetivo: sobreviver a falhas de link e de equipamento.
Pré-requisitos: Vol. 8.*

- [ ] `volumes/v14-alta-disponibilidade/01-vrrp.qmd` — VRRP: gateway redundante, prioridades, preempção.
- [ ] `volumes/v14-alta-disponibilidade/02-bonding.qmd` — Bonding: agregação de links (LACP, balance-xor) entre equipamentos.
- [ ] `volumes/v14-alta-disponibilidade/03-ecmp-pcc.qmd` — ECMP e load balancing PCC: múltiplos links de trânsito.
- [ ] `volumes/v14-alta-disponibilidade/04-failover-multilink.qmd` — Failover multi-link na prática: recursive routing, detecção real de queda.

## Volume 15 — Operação de ISP de ponta a ponta
*Objetivo: integrar tudo no desenho e na operação de um provedor real.
Pré-requisitos: Vols. 5, 7, 9, 11, 13 e 14.*

- [ ] `volumes/v15-operacao-isp/01-desenho-rede-provedor.qmd` — Desenho de rede do provedor: core/distribuição/acesso, endereçamento, gerência fora de banda.
- [ ] `volumes/v15-operacao-isp/02-fibra-olt-bng.qmd` — Topologia fibra: OLT + MikroTik como BNG (VLANs de serviço, PPPoE sobre PON).
- [ ] `volumes/v15-operacao-isp/03-radio-topologia-rural.qmd` — Topologia rádio rural completa: repetidoras, POPs solares, backhaul.
- [ ] `volumes/v15-operacao-isp/04-hardening-operacional-migracao-v7.qmd` — Hardening operacional (RoMON, upgrades, senhas de parque) e migração v6→v7.
- [ ] `volumes/v15-operacao-isp/05-estudo-de-caso-integrador.qmd` — Estudo de caso integrador: o provedor completo, do trânsito ao roteador do cliente.
