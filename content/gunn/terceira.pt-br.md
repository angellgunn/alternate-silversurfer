---
title: "Morte e Vida do MacBook 2011"
author: "agunn"
date: 2017-11-07T23:04:12-03:00
keywords: macbook pro, dGPU, arch linux, efi vars, force iGPU, hack of the year, foraTemer
---

Terceira postagem: meu bravo _macbook pro 15' early 2011_ morreu. Triste dia. Comprei-o da Apple Store nacional em dezembro de 2011. Ele já aguentara muita coisa, incluindo uma queda ao chão que lhe rendeu uma troca de display LCD. Em 2013, o famigerado problema da placa gráfica AMD (_discrete GPU, dGPU_) atingiu meu MBP pela primeira vez. Sem cobertura alguma de assistência técnica (não fiz a cobertura extendida, um erro, e na época ainda não havia iniciado o _recall_ da Apple), e fugindo dos preços estratosféricos cobrados pelos novos macbooks com tela retina (impossível adquirir um novo aquela altura), não tive escolha a não ser pagar o equivalente a US$800,00 por uma placa lógica nova.

Procurei a assistência técnica (sempre levo na mesma há anos, confio 100% neles), aguardei o interminável prazo de quase 1 mês para receberem a placa e logo estava de volta com meu MBP de guerra. Mas não dessa vez. Há quase 1 mês atrás, o problema da dGPU voltou de novo. Quatro anos sem mais problemas, foi um período bem maior do que a maioria relata após a troca da placa lógica. Mas, afinal, acabou. Levei na assistência autorizada da Apple (a de sempre), mas a máquina nem passou direito da porta. Os modelos de 2011 não tem mais suporte por serem considerados _obsoletos_ pela Apple, fui informado. Próxima tentativa: levei em uma assistência técnica _não autorizada_ (indicada pelo pessoal da autorizada). Passou alguns dias, recebi a confirmação por email de que se tratava do problema da dGPU e que _eles não poderiam resolver pois seu fornecedor não mandava mais peças de reposição para essa placa_. A Apple tinha dado um jeito definitivo de forçar a obsolescência programada? Enfim, há 4 dias, então, recebi de volta meu macbook pro, agora um peso de papel muito caro...

Minha próxima determinação foi: já que ninguém quer resolver, vou resolver eu mesmo! Vou adquirir a peça de segunda mão e trocar sozinho. Mesmo com a incerteza de talvez obter uma peça já com vida útil reduzida e sem saber ao certo se minha capacidade de _faça você mesmo_ seria suficiente, estava decicido. O primeiro passo foi pesquisar o Google. Como eu havia mexido no sistema operacional do MBP e tentato instalar um SO linux, procurei por alternativas que usassem uma distribuição linux no MBP. Para minha surpresa, caí [nessa](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/) discussão do fórum MacRumors. Iniciada por @AppleMacFinder em março deste ano, mostrava uma *solução 100% funcional* para ressuscitar um MBP 2011 com a placa gráfica defeituosa. O autor, aparentemente baseado na Rússia, mostrou uma forma simples e eficaz de forçar o MBP a isolar a dGPU e _funcionar apenas com a placa gráfica integrada à CPU Intel (iGPU)_. Não esperei mais e tentei de imediato.

O método:

1- Criei um disco de inicialização (CD/pendrive) do Arch Linux, escolhido por não ter interface gráfica. Fiz isso num notebook Windows 10.

2 - Iniciei o MBP pelo Arch Linux, segurando a tecla *Option* enquanto inicializava o MBP e escolhendo o disco (_EFI boot_) na tela que surge em seguida. Quando a inicialização começou, pressionei a tecla *E* para acessar o GRUB e editar a linha de comando que apareceu logo abaixo, adicionando `nomodeset`no final. Em seguida, continuei o processo pressionando *Enter*.

3- O SO iniciou e apresentou uma CLI do linux. Como o sistema de arquivos _efivarfs_ é montado por padrão, é possível editar as variáveis de EFI, primeiro remontando a unidade para permitir a gravação (este passo foi creditado ao usuário @totoe_84):

```bash
# cd /
# umount /sys/firmware/efi/efivars/
# mount -t efivarfs rw /sys/firmware/efi/efivars/
# cd /sys/firmware/efi/efivars/
```

4- Checando a pasta `/sys/firmware/efi/efivars`, se houver algum arquivo `gpu-power-prefs-<UUID>`, o mesmo deve ser removido. No meu caso, não havia tal arquivo.

5- Pulei algumas recomendações para o caso de problemas no display ou para eventualidade de encontrar outros arquivos de configuração (é bom checar com cuidado a discussão original) e parti para o ataque, criando uma nova configuração usando uma UUID qualquer (@AppleMacFinder tirou de uma placa física):

```bash
# printf "\x07\x00\x00\x00\x01\x00\x00\x00" > /sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9
# chattr +i "/sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9"
# cd /
# umount /sys/firmware/efi/efivars/
# reboot
```
Na discussão foram introduzidos outros métodos adicionais, mas inicialmente fiz apenas isso. _Como que por encanto, meu falecido MBP voltou à vida!_ Como eu havia apagado o SO nas inúmeras tentativas de autodiagnosticar o problema antes de levar na autorizada, usei a reinicialização pressionando as teclas *Option + Command + R* para acionar o _Internet Recovery_ da Apple. O procedimento funcionou perfeitamente (antes congelava a meio caminho) e baixou o SO mais novo (High Sierra) o qual foi instalado e reinicializou. Tudo perfeito! Ou quase. Ao percorrer as etapas de inicialização do High Sierra, quando o computador foi reiniciar ao final, ele congelou novamente. Após mais leituras da mesma discussão do fórum, descobri que seria necessário isolar um arquivo de configuração que tentava usar a dGPU da AMD. Para tanto, usei o [guia](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/page-35#post-24956091) do usuário @MikeyN, o qual faz algo muito semelhante que o método que usei primeiro, porém por outro caminho:

1- Resetei a SMC e NVRAM, pressionando *Option + Control + Shift esquerdo + Power*, liberando e, depois, iniciando com *Option + Command + P + R* pressionadas.

2- Após ouvir o som de inicialização duas vezes, pressionei *Command + R + S* para iniciar no modo _Recovery_. Em seguida, desabilitei o SIP e a dGPU:

```bash
# csrutil disable
# nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
# nvram boot-args="-v"
# reboot
```

3- Reiniciei no modo de usuário único, pressionando *Command + S*. Em seguida, montei a partição _root_ e movi o arquivo _infrator_ para uma pasta especialmente criada.

```bash
# /sbin/mount -uw /
# mkdir -p /System/Library/Extensions-off
# mv /System/Library/Extensions/AMDRadeonX3000.kext /System/Library/Extensions-off/
# touch /System/Library/Extensions/
# nvram boot-args=""
# reboot
```

Novamente, haviam alguns passos a mais para prevenir problemas que alguns usuários já observaram com o gerenciamento de energia e outros subsistemas. Mas até o momento não foi preciso adicionar mais nenhum _hack_ ao meu MBP. E pronto! Lá estava meu MBP, com sistema atualizado para High Sierra e pronto para uso! Já instalei VirtualBox, Docker, Ruby, Bundler, várias Gems, fiz donwload de meus repositórios Git, já gerenciei meus arquivos _dicom_ que tomam um mega-espaço, e até agora está tudo bem. Obrigado, @AppleMacFinder, @totoe_84, @MikeyN e todos os usuários do fórum MacRumors que contribuíram para criar esta engenhosa solução que permite continuar usando um MBP com defeito na placa gráfica.

Atualização (11/12/17): instalei a atualização 10.13.2 do High Sierra e isso parece ter revertido as alterações do _hack_. A instalação interrompeu a meio caminho e o macbook entrou em loop de reinício. Apenas segui todos os passos da segunda parte da descrição acima novamente. A instalação pôde continuar e o funcionamento do macbook voltou ao normal. Cada atualização do sistema trará o risco de quebrar novamente o _hack_, talvez de maneiras que não possam mais ser solucionadas com o método descrito. Assim, não farei mais atualizações. Recomendo a todos, porém, que façam a atualização para o [High Sierra 10.13.2](https://support.apple.com/pt-br/HT208331), pois ela conserta um grave erro de segurança detectado em 28/11/17, o qual permitia o acesso root a qualquer um, sem exigir senha (o.O).

Atualização (07/02/18): instalei a atualização 10.13.3 do High Sierra, recomendada para proteger o sistema da vulnerabilidade Spectre. O mesmo problema ocorreu e tive que aplicar o _hack_ novamente, tendo mais uma vez sucesso. Não planejo fazer mais nenhuma atualização, pelo risco de comprometer de vez o sistema.
