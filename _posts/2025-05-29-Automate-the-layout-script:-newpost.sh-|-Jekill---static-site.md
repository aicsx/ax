---
layout: post
title: "Automate the layout script: newpost.sh | Jekill - static site"
date: 2025-05-29 01:07:14 +0200
categories: blog
---

# Automate the layout for Jekill - Static Site "github"

> I was tired of creating my basic template by hand. For this reason I started to write a banal script in bash "simple and extremely powerful language from my point of view". I started with a basic code of 10 lines ... adding here and there some corrections and adjustments has become quite different. A trivial bash script for personal use has become a wiser awareness of what happens to be picky. Without going into too much detail, we could say that it is designed for Jekill - Static site, does not contain hidden code, it is clear, simple and effective. I share it to have an account or opinions on how to improve the concept of logic behind script writing or simply for utility transfer. I hope it is a pleasant thing and if it was not: PATIENCE, I will make a deal with it. Good to make!

{% highlight bash %}

~$ cat bash/newpost.sh

{% endhighlight %}

```bash
#!/bin/bash
# It was "simple" new post script for Jekill
# CC: ax at slackware dot eu
# IRC: ax [] 127.0.0.1, SLK/UmbrellaNet

set -f  # Disabilita globbing per evitare espansione indesiderata dei caratteri

# ----------- COLORI CATPPUCCIN -----------
COLOR_TITLE="\033[38;5;110m"
COLOR_INFO="\033[38;5;109m"
COLOR_WARNING="\033[38;5;174m"
COLOR_RESET="\033[0m"

# ----------- FUNZIONE: MESSAGGIO BREVE -----------
show_brief_message() {
  printf "%bNessun argomento passato.%b\n" "$COLOR_WARNING" "$COLOR_RESET"
  printf "Usa %b-h%b, %b--help%b per l'aiuto oppure %b-d%b, %b--desc%b per la descrizione.\n" \
    "$COLOR_TITLE" "$COLOR_RESET" "$COLOR_TITLE" "$COLOR_RESET" "$COLOR_TITLE" "$COLOR_RESET" "$COLOR_TITLE" "$COLOR_RESET"
}

# ----------- FUNZIONE: HELP -----------
show_help() {
  printf "%bUtilizzo:%b %s \"Titolo del post\" [directory_destinazione]\n\n" "$COLOR_TITLE" "$COLOR_RESET" "$0"

  printf "%bParametri disponibili:%b\n" "$COLOR_TITLE" "$COLOR_RESET"
  printf "  -d, --desc           Mostra la descrizione dettagliata.\n"
  printf "  -hd, --help --desc   Mostra sia help che descrizione.\n\n"

  printf "%bEsempi:%b\n" "$COLOR_TITLE" "$COLOR_RESET"
  printf "  %s \"%s\" posts/\n" "$0" "Appunti [lezione jazz] n.1"
  printf "  %s \"%s\"\n\n" "$0" "Un altro titolo"

  printf "%bATTENZIONE:%b\n" "$COLOR_WARNING" "$COLOR_RESET"
  printf "  Per usare una directory da creare, aggiungi sempre uno slash '/' finale.\n"
}

# ----------- FUNZIONE: DESCRIZIONE DETTAGLIATA -----------
show_description() {
  printf "%bDescrizione:%b\n" "$COLOR_TITLE" "$COLOR_RESET"
  printf "  - Il titolo del post deve essere racchiuso tra virgolette se contiene caratteri speciali\n"
  printf "    come: %b[] {} ! * ? °%b e spazi.\n" "$COLOR_TITLE" "$COLOR_RESET"
  printf "  - Se non specifichi la %bdirectory_destinazione%b, il file sarà creato nella directory corrente.\n" "$COLOR_TITLE" "$COLOR_RESET"
  printf "  - Se specifichi una %bdirectory esistente%b come ultimo argomento, il file sarà creato lì.\n" "$COLOR_TITLE" "$COLOR_RESET"
  printf "  - Se la directory specificata %bNON%b esiste ma termina con uno slash '/', lo script chiederà\n" "$COLOR_WARNING" "$COLOR_RESET"
  printf "    se vuoi crearla e poi la userà come destinazione.\n"
  printf "  - Se l'ultimo argomento non è una directory o non termina con '/', sarà considerato\n"
  printf "    parte del titolo.\n\n"
}

# ----------- GESTIONE OPZIONI -----------
if [[ $# -eq 0 ]]; then
  show_brief_message
  exit 0
fi

if [[ "$1" == "-hd" || ( "$1" == "--help" && "$2" == "--desc" ) ]]; then
  show_help
  show_description
  printf "\n%bBuon lavoro!%b\n" "$COLOR_INFO" "$COLOR_RESET"
  exit 0
elif [[ "$1" == "-h" || "$1" == "--help" ]]; then
  show_help
  printf "\n%bBuon lavoro!%b\n" "$COLOR_INFO" "$COLOR_RESET"
  exit 0
elif [[ "$1" == "-d" || "$1" == "--desc" ]]; then
  show_description
  printf "\n%bBuon lavoro!%b\n" "$COLOR_INFO" "$COLOR_RESET"
  exit 0
fi

# ----------- RILEVAZIONE TITOLO E DIRECTORY -----------
last_arg="${!#}"

if [[ "$last_arg" == */ ]]; then
  dir_dest="${last_arg%/}"
  titolo="${*:1:$(($#-1))}"
else
  titolo="${*:1}"
  dir_dest="."
fi

# ----------- CONTROLLO / CREAZIONE DIRECTORY -----------
if [[ ! -d "$dir_dest" ]]; then
  printf "%bLa directory '%s' non esiste. Vuoi crearla? (y/N): %b" "$COLOR_WARNING" "$dir_dest" "$COLOR_RESET"
  read -r risposta
  case "$risposta" in
    y|Y|Yes|YES|yes)
      mkdir -p "$dir_dest" && printf "%bDirectory creata con successo.%b\n" "$COLOR_INFO" "$COLOR_RESET" || {
        printf "%bErrore nella creazione della directory.%b\n" "$COLOR_WARNING" "$COLOR_RESET"
        exit 1
      }
      ;;
    *)
      printf "%bOperazione annullata.%b\n" "$COLOR_WARNING" "$COLOR_RESET"
      exit 1
      ;;
  esac
fi

# ----------- GENERAZIONE DEL NOME FILE -----------
data=$(date +"%Y-%m-%d")
nome_file=$(echo "$titolo" | sed -e 's/ /-/g' -e 's/\[//g' -e 's/\]//g')
nome_file="$data-$nome_file.md"
full_path="$dir_dest/$nome_file"

# ----------- CREAZIONE FILE -----------
cat <<EOF > "$full_path"
---
layout: post
title: "$titolo"
date: $(date +"%Y-%m-%d %H:%M:%S %z")
categories: blog
---
EOF

# ----------- MESSAGGIO DI SUCCESSO -----------
if [[ $? -eq 0 ]]; then
  printf "%bFile creato con successo:%b %s\n" "$COLOR_INFO" "$COLOR_RESET" "$full_path"
else
  printf "%bErrore nella creazione del file.%b\n" "$COLOR_WARNING" "$COLOR_RESET"
  exit 1
fi

# ----------- MESSAGGIO FINALE -----------
printf "%bBuon lavoro!%b\n" "$COLOR_INFO" "$COLOR_RESET"
```

> Run the script with -h, -help and -d, --desc parameters to check for options. This is!

---

(END)

