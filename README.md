# 😎 Introduction

C'est donc à nouveau cette période de l'année, une nouvelle version de macOS est sortie et la question séculaire sera à nouveau posée : quels GPU sont pris en charge avec macOS Monterey ? Eh bien, vous êtes au bon endroit, nous allons donner un aperçu rapide de la situation et entrer plus en détail sur les GPU exacts que nous recommandons?

## Un rappel rapide avec NVIDIA et les pilotes Web

![](.gitbook/assets/WebDrivers.gif)

Eh bien, au moment de la rédaction de cet article, nous avons effectué plusieurs cycles de système d'exploitation sans pilotes officiels de NVIDIA pour leurs GPU Maxwell, Pascal, Turing ou Ampere. Cela signifie que les utilisateurs de ces GPU ne prennent pas en charge Mojave ou une version ultérieure, ils sont donc bloqués avec macOS High Sierra. Qui est à blâmer ? Eh bien, ce sont 2 entreprises géantes et égoïstes qui refusent toutes les deux de travailler ensemble, donc le blâme peut aller dans les deux sens. Gardez à l'esprit que les WebDrivers ont un problème de fuite de VRAM qu'ils n'ont pas encore résolu, donc une théorie expliquant pourquoi Apple refuse les pilotes Nvidia dans macOS peut être due à la façon dont Nvidia refuse de remettre la pile de pilotes. Vous pensez que c'est une coïncidence si AMD et Intel ont des pilotes open source ? Eh bien, de toute façon, cela ne change rien au fait qu'il n'y a pas de soutien.

Kepler a été abandonné dans macOS Monterey, ce qui signifie qu'il n'y a plus de support pour les GPU NVIDIA.

Et pour ceux qui veulent un peu de lecture à faire : [When will the NVIDIA Web Drivers be released for macOS Mojave 10.14](https://devtalk.nvidia.com/default/topic/1042520/drivers/-when-will-the-nvidia-web-drivers-be-released-for-macos-mojave-10-14-/post/5358999/#5358999)

Excellente lecture car elle montre comment même la haute direction n'a pas de bonne réponse pour les clients.

## Donc si mon GPU est supporté nativement, pourquoi ai-je besoin de Lilu et de WhateverGreen ?

Cette question revient assez souvent dans la communauté Hackintosh, et pour cause. Pourquoi diable ces GPU fonctionneraient-ils prêts à l'emploi sur un Mac et non sur un Hackintosh ? Eh bien, la raison en est que les PC et les Mac ont un câblage interne différent et que les dispositions ACPI d'un PC ne fonctionnent pas bien avec macOS dans différents scénarios. Pour contourner ce problème, nous utilisons [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases) et son compagnon [Lilu](https://github.com/acidanthera/Lilu/releases) pour patcher différentes parties de notre Hackintosh, comme renommer les appareils, aider aux connexions de framebuffer, patcher les connecteurs audio, permettre des modifications à aty\_config, aty\_properties, cail\_properties via ACPI et bien plus encore. Avec un ensemble de fonctionnalités aussi vaste et développé par quelqu'un qui sait ce qu'il fait, il n'y a aucune raison de ne pas l'utiliser.

## Alors, quelles sont les options?

macOS Monterey a abandonné la prise en charge de tous les GPU NVIDIA, y compris les cartes basées sur Kepler, ce qui signifie que les GPU dédiés AMD et les GPU intégrés Intel sont la seule solution pour la dernière version.&#x20;

Choses à retenir:

* macOS ne prend pas en charge SLI, Crossfire ou les GPU avec plusieurs cœurs principaux (comme la Radeon Pro Duo). Cela pourrait changer avec la sortie de la Radeon Pro Vega II Duo sur Mac Pro.
* Obtenir de l'audio via HDMI/DisplayPort peut nécessiter un travail supplémentaire avec AppleALC.kext et d'autres modifications IOReg
* L'overclocking GPU est limité aux GPU Vega 10 avec [PyVega](https://github.com/corpnewt/PyVega)
* L'exécution de GPU pris en charge aux côtés de GPU non pris en charge peut avoir des conséquences étranges, car les GPU non pris en charge fonctionnent avec des pilotes VESA qui ont le problème d'interrompre le sommeil et d'autres fonctions dans macOS. Veuillez vous référer à [Désactivation des GPU](https://dortania.github.io/OpenCore-Install-Guide/extras/spoof.html) pour plus d'informations.

## Puis-je exécuter un GPU non pris en charge dans mon hack ?

Donc, quelque chose à garder à l'esprit lors de l'exécution d'un GPU non pris en charge dans macOS est de se rabattre sur les pilotes VESA lorsqu'aucun pilote réel n'est présent. Ce sont des pilotes très simples basés sur le processeur qui sont utilisés comme un palliatif pendant que vous attendez pour installer les bons pilotes, mais de nombreuses fonctions de base de macOS sont interrompues lors de son exécution, y compris la veille et la stabilité générale. Et comme ces GPU n'ont pas de pilotes, même en dehors d'Apple, nous avons besoin d'un moyen d'empêcher la reconnaissance du GPU non pris en charge dans macOS. Consultez le guide [Désactivation des GPU](https://dortania.github.io/OpenCore-Install-Guide/extras/spoof.html) pour plus d'informations à ce sujet.

\>Mais puis-je rendre macOS sur mon iGPU mais utiliser les sorties vidéo sur mon GPU non pris en charge ?

Malheureusement non, et la raison est en fait assez similaire au fonctionnement de la technologie Optimus de NVIDIA. Vous auriez d'abord besoin d'un moyen de saisir/encoder le signal de l'iGPU, de l'envoyer vers le GPU discret, puis de faire en sorte que ledit GPU décode le signal et l'affiche. Un petit problème, le décodage du signal nécessiterait une accélération GPU appropriée que votre GPU non pris en charge n'a pas. Vous devrez donc utiliser les ports de sortie vidéo de votre carte mère quoi qu'il arrive.
