# 2. Installer la CLI

Les commandes Bash ci-dessous s'appliquent à macOS et Linux. Sous Windows, consultez d'abord le [parcours Windows](01a-windows.md) : le lanceur et les vérifications natifs ne sont pas encore validés.

Clonez la CLI officielle sur votre machine, dans un dossier que vous conserverez en dehors de vos projets :

~~~bash
mkdir -p "$HOME/.local/bin"
git clone --branch main https://github.com/Symsvel-Framework/symsvel-cli.git "$HOME/.local/symsvel-cli"
chmod +x "$HOME/.local/symsvel-cli/bin/symsvel"
ln -s "$HOME/.local/symsvel-cli/bin/symsvel" "$HOME/.local/bin/symsvel"
export PATH="$HOME/.local/bin:$PATH"
symsvel doctor
~~~

Ajoutez la ligne export à la configuration de votre shell (par exemple ~/.zshrc ou ~/.bashrc) pour rendre la commande disponible dans les nouveaux terminaux.

Si doctor signale un prérequis manquant, installez-le puis relancez la commande. La CLI Symfony est facultative.

Pour comprendre les commandes et les responsabilités de la CLI, consultez la [référence CLI](../06-reference/01-cli.md).

[Étape suivante : créer un projet](03-creer-un-projet.md).

Les catalogues officiels de distribution sont `https://packages.symsvel.com` (stable), `https://packages-dev.symsvel.com` (développement) et `https://recipes.symsvel.com` (recettes Flex).
