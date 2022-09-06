---
title: "Session 002"
tags:
- timeline
- session
date: 2022-09-06 03:44
---
<span 
		class='ob-timelines'
		data-date="2022-09-06-00">
</span>

👥 [**Reference meeting: 002**](notes/meetings/meeting%20002.md)

🔙 [**Previous work session: 001**](notes/sessions/session%20001.md)

> [!info]+ My goals for this session
> 
> * prima lettura del dataset per capire cosa contiene
> * primi esperimenti con il codice


## Dataset 
Ho iniziato a farmi un'idea dei dati che il software dovrà validare a partire dai dati di Crossref trasformati in CSV di input per OC Meta([dataset online su cartella onedrive](https://liveunibo-my.sharepoint.com/personal/arcangelo_massari_unibo_it/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Farcangelo%5Fmassari%5Funibo%5Fit%2FDocuments%2FDocuments%2Fmeta%5Finput%2Ezip&parent=%2Fpersonal%2Farcangelo%5Fmassari%5Funibo%5Fit%2FDocuments%2FDocuments&ga=1)).

Sono partito dall'esaminare gli identificativi degli autori; finora emerge che sono presenti *almeno* tre tipi di identificativi: `doi`, `isbn` e `issn`, dal più frequente al meno frequente.



## Codice

Esiste già tra i lavori di OC (https://github.com/opencitations/index/tree/master/index/identifier) il codice per verificare la correttezza sintattica di un identificativo e la sua esistenza, e se necessario normalizzarlo, *almeno* per `doi`, `issn` e `orcid`. 
Se non esiste già, andrebbe quindi fatto un programma, sul modello di quelli per `doi`, `issn` e `orcid`,  che gestisca gli `isbn`.

Ho riutilizzato questo codice all'interno del file [session002.py](https://github.com/eliarizzetto/index/blob/master/index/validation/session002.py) che per ora permette di:
* accedere a e leggere tutti i file .csv all'interno di una cartella di input e all'interno di sue sottocartelle
* leggere i file come dizionari
* verificare la validità del valore/dei valori del field `"id"` con una richiesta alla relativa API, per `doi` e `issn`, sfruttando il codice preesistente
* nel caso in cui l'id non sia valido, mandare a schermo un messaggio con la posizione dell'errore

È da definire meglio la *condizione* attraverso cui si sceglie quale funzione chiamare per analizzare ciascun valore di id: per ora c'è solo una regex minimale che controlla l'inizio del valore (ammette solo i casi in cui non ci sono anomalie nel prefisso).

Questo il codice aggiornato a 06/09 (e qui la [versione online](https://github.com/eliarizzetto/index/commit/e5967776e216b0c993fce368b5475c3b611d6c2c#diff-ce0a579d2aa9204ad6e68331df3f331c9e515cfb7ba3a98b43fb7b7e1c0e48c5)):

```python {title="session002.py"}
# ----accessing and reading files  
import glob  
from csv import DictReader  
from os import sep, makedirs, walk  
from os.path import exists, basename, isdir  
import tarfile  
  
# ----managing identifiers  
  
from index.identifier.doimanager import DOIManager  
from index.identifier.issnmanager import ISSNManager  
from index.identifier.orcidmanager import ORCIDManager  
from re import sub, match  
  
  
source_paths = 'C:/Users/media/Desktop/thesis23/meta_input'  
#source_paths= "C:/Users/media/Desktop/thesis23/oc/index/index/validation/test_files"  
  
  
def get_all_files(i_dir_or_targz_file):  
    """  
    Modified version of index.coci.glob.get_all_files() in order to access .csv files. If compressed files are found,    it automatically decompress and read them. Returns 2 variables: a list of file paths of raw .csv files; and the    list of decompressed files if any is found (otherwise None).    """    result = []  
    targz_fd = None  
  
    if isdir(i_dir_or_targz_file):  
        for cur_dir, cur_subdir, cur_files in walk(i_dir_or_targz_file):  
            for cur_file in cur_files:  
                if cur_file.endswith(".csv") and not basename(cur_file).startswith("."):  
                    result.append(cur_dir + sep + cur_file)  
    elif i_dir_or_targz_file.endswith("tar.gz"):  
        targz_fd = tarfile.open(i_dir_or_targz_file, "r:gz", encoding="utf-8")  
        for cur_file in targz_fd:  
            if cur_file.name.endswith(".csv") and not basename(cur_file.name).startswith("."):  
                result.append(cur_file)  
    else:  
        print("It is not possible to process the input path.")  
    return result, targz_fd  
  
  
  
  
def access_csv(source_dir):  
  
    """Accesses the input directory and its relative subdirectories  
    and returns a list of all the .csv file paths therein"""  
    source_data = glob.glob(f'{source_dir}/**/*.csv', recursive=True)  
    return source_data  
  
  
  
# ----------------parametri da mettere in input: source_paths,  
  
doi_mngr = DOIManager()  
issn_mngr = ISSNManager()  
  
  
for file in access_csv(source_paths):  
    with open(file, 'r', encoding='utf-8') as source_file:  
        data_dict = DictReader(source_file)  
  
        for position_in_table, row in enumerate(data_dict):  
  
            field_id = row['id']  
            #print(field_id)  
  
            for position_in_field, id_instance in enumerate(field_id.split()):  
                print(id_instance)  
  
                if match("^doi:10", id_instance):  
                    normalized_id_instance = doi_mngr.normalise(id_instance)  
  
                    if doi_mngr.is_valid(normalized_id_instance) is None:  
                        print(f'Take a look at id no. {position_in_field} at row no. {position_in_table} in file {file}')  
  
                if match("^issn:", id_instance):  
                    normalized_id_instance = issn_mngr.normalise(id_instance)  
                    if issn_mngr.is_valid(normalized_id_instance) is None:  
                        print(f'Take a look at id no. {position_in_field} at row no. {position_in_table} in file {file}')  
  
                if match("^isbn:", id_instance):  
                    print(f'Take a look at id no. {position_in_field} at row no. {position_in_table} in file {file}')  
                    print("A ISBN manager is yet to be developed")
```
