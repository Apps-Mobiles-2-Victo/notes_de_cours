# 05 - Données persistantes - Données distantes (API) - 2e partie

## STROTTIER - pas présenté dans le cours A2026 (Apps Mobiles 2 - 60h)

## 72.3 Synchroniser les données locales avec les données distantes


Une application mobile qui travaille avec des données locales a tout avantage à synchroniser ses données avec une base de données distante afin de s'assurer de ne rien perdre en cas de bris du téléphone.


En temps normal, l'application effectuera chacune des opérations avec la base de données locales ET avec la base de données distante.


Mais dans le cas où l'application roule alors qu'elle n'a pas accès à Internet, seules les données locales pourront être modifiées. Il faut donc mettre en place un mécanisme qui se chargera d'effectuer ces modifications sur la base de données distante dès que l'accès à Internet sera retrouvé.


Il est possible de programmer une application Android avec Jetpack Compose pour qu'elle réagisse lorsqu'elle détecte un changement de la connectivité : [https://blog.devgenius.io/monitoring-internet-connection-on-android-jetpack-writing-e007b3d61915](https://blog.devgenius.io/monitoring-internet-connection-on-android-jetpack-writing-e007b3d61915)


### 72.4 Service Web pour synchroniser les données


Je vous démontre ici comment écrire un service Web en PHP qui interagit avec une base de données MySQL afin de recopier des données locales dans une base de données distante.


Ce service peut être utilisé avec une application mobile pour iOS ou pour Android de même qu'avec tout autre type d'application qui utilise des données locales.


Dans cette fiche :


#### Synchronisation vs enregistrement au fur et à mesure dans la base de données distante


#### Serveurs de développement


#### Structure des informations envoyées au service Web puis retournées par le service Web


#### Sécurité et autorisations


#### Branchement à la base de données


#### Comparer les enregistrements des BD locale et distante


#### UUID ou ULID


#### Une bonne base pour votre service Web


#### Tester le service Web manuellement


#### Consommer le service Web


### Synchronisation vs enregistrement au fur et à mesure dans la base de données distante


Quand on travaille avec une base de données locale et une base de données distante, il faut distinguer deux figures de cas :


#### les modifications sont effectuées sur la base de données locale et sur la base de données distante au fur et à mesure


#### les modifications sont effectuées seulement sur la base de données locales puis, à un moment précis, les données
locales sont synchronisées avec les données distantes.


Ces deux figures de cas sont complémentaires.


En temps normal, l'application utilisera la première approche : chacune des opérations sera effectuée avec la base de données locales ET avec la base de données distante.


Mais dans le cas où l'application roule alors qu'elle n'a pas accès à Internet, seules les données locales pourront être modifiées.


C'est là qu'entre en jeu la synchronisation. Elle permet de comparer les données locales et les données distantes afin d'effectuer les opérations d'ajout, de modification et de suppression qui n'ont pas encore été effectuées sur les données distantes.


Dans cette fiche, je vous montre comment développer un service Web qui permettra d'effectuer cette synchronisation.


### Serveurs de développement


Pour effectuer la copie des données locales, il faut que l'application mobile ait accès à un service Web qui interagira avec la base de données distante.


Pendant la phase de développement de votre application, le service Web peut tourner localement. Vous aurez besoin d'un serveur HTTP et d'un serveur de bases de données.


Ces serveurs peuvent être installés sur votre ordinateur à l'aide d'un environnement de développement Web tel que Devilbox , un outil qui préconfigure des conteneurs **Docker** qui font rouler Apache ou Ngnix, MySQL, etc.


Quand vous aurez terminé le développement et la phase de tests de votre service Web, vous pourrez le mettre en ligne chez un hébergeur, comme vous le feriez pour un site Web.


### Structure des informations envoyées au service Web puis retournées par le service Web


Le format **JSON** est très utilisé pour échanger des données entre applications.


L'application mobile doit fournir au service Web une représentation JSON des données à synchroniser.


De son côté, le service Web recevra ces données  et il s'en servira dans son traitement.


Il  remplira un tableau associatif avec les informations qu'il souhaite fournir en retour à l'application mobile.


À la fin du traitement, le service convertira ce tableau au format JSON puis il fera un echo de cette valeur. C'est ce echo qui sera la valeur de retour du service Web.


L'application mobile pourra lire l'information retournée et réagir en conséquence.


```kotlin title="Fichier monservice/synchro-clients.php"
...
$tableauRetour = [...];
// Retrouve les données envoyées par l'application mobile.
$jsonBrut = file_get_contents('php://input');    // $_POST ne fonctionne que pour les Content-Type application/x-www-form-
urlencoded ou multipart/form-data
if ($jsonBrut == null) {
    $tableauRetour ['erreurs'][] = ['code' => 5, 'message' => "Aucune donnée locale à synchroniser n'a été reçue."];
}
else {
    $donnees = json_decode ($jsonBrut);
    ...
    $tableauRetour  ...;
}
// Retourne les informations à l'application mobile.
// Remarquez que les paramètres JSON_PRETTY_PRINT, JSON_UNESCAPED_UNICODE et JSON_UNESCAPED_SLASHES assurent les caractères
spéciaux seront correctement encodés.
echo json_encode ($tableauRetour, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES);
```


### Sécurité et autorisations


Tout service Web qui manipule des données doit se soucier des problèmes de sécurité.


Entre autres, il faut mettre en place un mécanisme d'authentification qui assurera que seules les applications autorisées peuvent faire appel au service Web.


Les concepts de l'authentifications auprès d'un service Web sont expliqués dans la fiche « **l_authentification_aupres_du_service_web** ».


### Branchement à la base de données


Le service Web doit réussir à se brancher à la base de données.


En cas d'erreur, il se chargera d'initialiser un élément du tableau associatif afin de laisser savoir qu'il y a eu un problème.


```kotlin title="Fichier monservice/synchro-clients.php (PHP 8.x)"
$serveurBD='127.0.0.1';
$usagerBD = 'root';
$motDePasseBD = '';
$nomBD = 'mabd';
// Branchement à la base de données.
$continuer = false;
try {
    $mysqli = new mysqli($serveurBD, $usagerBD, $motDePasseBD, $nomBD);
    $mysqli->set_charset("utf8mb4");
    $continuer = true;
    ...
} catch (Exception $e) {
    $tableauRetour['erreurs'][] = ['code' => 2, 'message' => "Échec lors de la
connexion à la base de données."];
}
if ($continuer) {
    ...
}
```


### Comparer les enregistrements des BD locale et distante


Il faut distinguer trois figures de cas :


#### ajout : l'enregistrement n'est pas encore dans la BD distante. Il est seulement dans la BD locale.


#### modification : l'enregistrement est dans la BD distante mais ses données sont différentes de celles de la BD locale.


#### suppression : l'enregistrement est encore dans la BD distante alors qu'il a été supprimé de la BD locale.


À première vue, on pourrait utiliser l'identifiant d'un enregistrement pour comparer sa présence dans la BD locale et dans la BD distante.


Le problème, c'est qu'en cas d'ajout, il faudrait forcer l'identifiant afin d'assurer que les deux bases de données puissent demeurer synchronisées. Ceci empêcherait la synchronisation à partir de plusieurs applications différentes puisque chacune pourrait faire un ajout local avec le même identifiant.


### UUID ou ULID


Pour permettre la synchronisation à partir de plusieurs sources, il est possible d'utiliser un **identifiant unique universel** (Universally unique identifier, UUID) comme valeur de base pour la synchronisation.


L'utilisation d'un **ULID** (Universally Unique Lexicographically sortable IDentifier) est également possible.


Ici, le UUID a été utilisé.


Le UUID est une chaîne hexadécimale au format aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee.


Il faut donc ajouter un champ à chacune des tables et s'assurer de le remplir adéquatement.


Plusieurs langages et SGBD permettent de générer un UUID par programmation :


#### Swift : UUID().uuidString


#### Kotlin :  UUID.randomUUID()


#### MySQL : UUID()


Pour remplir manuellement ce champ dans une base de données existante, je vous propose trois techniques simples.


### Terminal


Il est très simple de générer un UUID dans le Terminal macOS à l'aide de la commande uuidgen .


```kotlin title="Terminal"
uuidgen
```


### Site Web générateur de UUID


Vous pouvez également travailler à partir d'un site générateur de UUID, par exemple [https://www.uuidgenerator.net](https://www.uuidgenerator.net) .


### Avec MySQL


Ouvrez un éditeur MySQL, par exemple phpMyAdmin, puis exécutez la requête :


```kotlin title="MySQL"
SELECT UUID();
```


### Une bonne base pour votre service Web


Je vous propose ici une première version du service Web que vous pouvez adapter pour vos besoins.


Cette version est perfectible et se veut un simple départ pour éviter de tout construire à partir de zéro.


```kotlin title="Fichier monservice/synchro-clients.php (PHP 8.x)"
<?php
/**
 * Synchronisation à sens unique des données locales vers MySQL.
 *
 * L'application qui consomme ce service Web doit fournir des données par POST au
format :
 * [
 *     {"uuid": "...", "prenom": "...", "nomfamille": "..."},
 *     {"uuid": "...", "prenom": "...", "nomfamille": "..."}
 * ]
 *
 * @author Christiane Lagacé <christianelagace.com>
 *
 * @return String chaîne JSON au format :
 * {
 *     "erreurs" : [
 *         {"code" : 99, "message" : "..."},
 *         {"code" : 99, "uuid" : "...", "message" : "..."}
 *     ],
 *     "ajouts" : ["UUID1", "UUID2", ...],
 *     "modifications" : ["UUID3", "UUID4", ...],
 *     "suppressions" : ["UUID5", "UUID6", ...],
 *     "jwt" : "..."
 * }
 *
 * Codes d'erreurs :  1 : Accès refusé.
 *                    2 : Échec lors de la connexion à la base de données.
 *                    3 : Un problème empêche de vérifier les informations
d'authentification.
 *                    4 : Les informations d'authentification ne sont pas valides ou
le jeton est expiré.
 *                    5 : Aucune donnée locale à synchroniser n'a été reçue.
 *                    6 : Il n'est pas possible de synchroniser les ajouts et les
modifications.
 *                    7 : Il n'est pas possible de vérifier s'il y a des
enregistrements à supprimer dans la base de données distante.
 *                    8 : L'ajout d'un enregistrement a échoué.
 *                    9 : La mise à jour d'un enregistrement a échoué.
 *                   10 : La suppression d'un enregistrement a échoué.
 */
// Configurations
// *****************************************************************
$serveurBD='127.0.0.1';
$usagerBD = 'root';
$motDePasseBD = '';
$nomBD = 'mabd';
// *** Fin configurations ******************************************
// Le dossier du fichier journal (log) doit exister au même niveau que le dossier du
service Web.
$dossierRacineServeur = dirname(__FILE__, 2);
define('LOG_FILE', $dossierRacineServeur . DIRECTORY_SEPARATOR . 'log' .
DIRECTORY_SEPARATOR . 'apifactures.log');
$messageAccesRefuse = "Accès refusé.";
$codeAccesRefuse = 1;
$messageErreurConnexion = "Échec lors de la connexion à la base de données.";
$codeErreurConnexion = 2;
$messageErreurVerifierAuthentification = "Un problème empêche de vérifier les informations d'authentification.";
$codeErreurVerifierAuthentification = 3;
$messageErreurInformationsAuthentification = "Les informations d'authentification ne sont pas valides ou le jeton est
expiré.";
$codeErreurInformationsAuthentification = 4;
$messageErreurPost = "Aucune donnée locale à synchroniser n'a été reçue.";
$codeErreurPost = 5;
$messageErreurSynchroAjout = "Il n'est pas possible de synchroniser les ajouts et les modifications.";
$codeErreurSynchroAjout = 6;
$messageErreurSynchroSuppression = "Il n'est pas possible de vérifier s'il y a des enregistrements à supprimer dans la base de
données distante.";
$codeErreurSynchroSuppression = 7;
$messageErreurAjout = "L'ajout d'un enregistrement a échoué.";
$codeErreurAjout = 8;
$messageErreurMiseAJour = "La mise à jour d'un enregistrement a échoué.";
$codeErreurMiseAJour = 9;
$messageErreurSuppression = "La suppression d'un enregistrement a échoué.";
$codeErreurSuppression = 10;
$tableauRetour = [
    'erreurs' => [],
    'ajouts' => [],
    'modifications' => [],
    'suppressions' => [],
    'jwt' => ''
];
// Branchement à la base de données
// *****************************************************************
try {
    $mysqli = new mysqli($serveurBD, $usagerBD, $motDePasseBD, $nomBD);
    $mysqli->set_charset("utf8mb4");
    $continuer = true;
} catch (Exception $e) {
    $continuer = false;
    log_error($messageErreurConnexion);
    $tableauRetour['erreurs'][] = ['code' => $codeErreurConnexion,'message' => $messageErreurConnexion];
}
if ($continuer) {
    // Récupération des données envoyées par l'application mobile
    // *****************************************************************
    $jsonBrut = file_get_contents('php://input'); // $_POST ne fonctionne que pour les Content-Type application/x-www-form-
urlencoded ou multipart/form-data
    if ($jsonBrut == null) {
         log_error($messageErreurPost);
         $tableauRetour['erreurs'][] = ['code' => $codeErreurPost, 'message' => $messageErreurPost];
    }
    else {
        $clientsSqlite = json_decode($jsonBrut);   // liste des clients dans la BD SQLite
        //log_info("Données reçues :");
        //log_info($clientsSqlite);
        // Vérification des droits
        // *****************************************************************
        // ...
        $tableauRetour['jwt'] = "...";
        // Recherche des enregistrements à supprimer
        // *****************************************************************
        $requete = "SELECT uuid, nomfamille, prenom FROM clients";
        try {
            $resultat = $mysqli->query($requete);
            if ($mysqli->affected_rows > 0) {
                while ($enreg = $resultat->fetch_row()) {
                    // L'enregistrement n'est pas dans SQLite : on le supprime.
                    // *****************************************************************
                    if (!presentDansTableauDObjets($enreg[0], $clientsSqlite, 'uuid')) {
                        if (suppressionClient($enreg[0], $enreg[1], $enreg[2])) {
                            $tableauRetour['suppressions'][] = $enreg[0];
                        }
                    }
                }
            }
            $resultat->free();
        } catch (Exception $e) {
            log_error("$messageErreurSynchroSuppression - $mysqli->error");
            $tableauRetour['erreurs'][] = ['code' => $codeErreurSynchroSuppression, 'message' =>
$messageErreurSynchroSuppression];
        }
        // Recherche des enregistrements à ajouter ou à modifier
        // *****************************************************************
        $requete = "SELECT prenom, nomfamille FROM clients WHERE uuid = ?";
        try {
            $stmt = $mysqli->prepare($requete);
            foreach($clientsSqlite as $clientSqlite) {
                 $stmt->bind_param('s', $clientSqlite->uuid);
                 $stmt->execute();
                 $stmt->store_result();
                if ($stmt->errno != 0) {
                     log_error("$messageErreurSynchroAjout - uuid: $clientSqlite->uuid - nom: $clientSqlite->nomfamille -
prenom: $clientSqlite->prenom - stmt->error");
                     $tableauRetour['erreurs'][] = ['code' => $codeErreurSynchroAjout, 'uuid' => $clientSqlite->uuid,
'message' => $messageErreurSynchroAjout];
                 }
                 else {
                     // L'enregistrement existait dans la BD distante.
                     // *****************************************************************
                     if ($stmt->num_rows > 0) {
                         $stmt->bind_result($prenom, $nomfamille);
                         $stmt->fetch();
                         // L'enregistrement est différent : on fait la mise à jour.
                         // *****************************************************************
                         if ($prenom != $clientSqlite->prenom || $nomfamille != $clientSqlite->nomfamille) {
                             if (miseAJourClient($clientSqlite->uuid, $clientSqlite->prenom, $clientSqlite->nomfamille)) {
                                 $tableauRetour['modifications'][] = $clientSqlite->uuid;
                             }
                         }
                     }
                     else {
                         // L'enregistrement n'existait pas : on l'ajoute.
                         // *****************************************************************
                         if (ajoutClient($clientSqlite->uuid, $clientSqlite->prenom, $clientSqlite->nomfamille)) {
                             $tableauRetour['ajouts'][] = $clientSqlite->uuid;
                         }
                     }
                 }
            }
            $stmt->close();
        } catch (Exception $e) {
             log_error("$messageErreurSynchroAjout - $mysqli->error");
             $tableauRetour['erreurs'][] = ['code' => $codeErreurSynchroAjout, 'message' => $messageErreurSynchroAjout];
        }
    }
}
//log_info("Informations retournées :");
//log_info($tableauRetour);
// Retour des informations à l'application mobile
// *****************************************************************
// Remarquez que les paramètres JSON_PRETTY_PRINT, JSON_UNESCAPED_UNICODE et JSON_UNESCAPED_SLASHES assurent les caractères
spéciaux seront correctement encodés.
echo json_encode($tableauRetour, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES);
/**
 * Met à jour le client dans la BD distante selon son UUID.
 *
 * @param String $uuid       Identifiant unique universel du client.
 * @param String $prenom     Prénom à enregistrer.
 * @param String $nomfamille Nom de famille à enregistrer.
 *
 * @author Christiane Lagacé <christianelagace.com>
 *
 * @return bool True si l'opération a réussi.
 *
 */
function miseAJourClient($uuid, $prenom, $nomfamille) {
    global $mysqli;
    global $messageErreurMiseAJour;
    global $codeErreurMiseAJour;
    global $tableauRetour;
    $retour = false;
    $requete = "UPDATE clients SET prenom = ?, nomfamille = ? WHERE uuid = ?";
    try {
        $stmt = $mysqli->prepare($requete);
        $stmt->bind_param('sss', $prenom, $nomfamille, $uuid);
        $stmt->execute();
        if (0 == $stmt->errno) {
            $retour = true;
        }
        else {
            log_error("$messageErreurMiseAJour - uuid: $uuid - nom: $nomfamille - prenom: $prenom - $stmt->error");
            $tableauRetour['erreurs'][] = ['code' => $codeErreurMiseAJour, 'uuid' => $uuid, 'message' =>
$messageErreurMiseAJour];
        }
        $stmt->close();
    } catch (Exception $e) {
        log_error("$messageErreurMiseAJour - uuid: $uuid - nom: $nomfamille - prenom: $prenom - $mysqli->error");
        $tableauRetour['erreurs'][] = ['code' => $codeErreurMiseAJour, 'uuid' => $uuid, 'message' => $messageErreurMiseAJour];
    } catch (Error $e) {
        log_error("$messageErreurMiseAJour - uuid: $uuid - nom: $nomfamille - prenom: $prenom - " . $e->getMessage());
        $tableauRetour['erreurs'][] = ['code' => $codeErreurMiseAJour, 'uuid' => $uuid, 'message' => $messageErreurMiseAJour];
    }
    return $retour;
}
/**
 * Ajoute un client dans la BD distante.
 *
 * @param String $uuid       Identifiant unique universel à enregistrer.
 * @param String $prenom     Prénom à enregistrer.
 * @param String $nomfamille Nom de famille à enregistrer.
 *
 * @author Christiane Lagacé <christianelagace.com>
 *
 * @return bool True si l'opération a réussi.
 *
 */
function ajoutClient($uuid, $prenom, $nomfamille) {
    global $mysqli;
    global $messageErreurAjout;
    global $codeErreurAjout;
    global $tableauRetour;
    $retour = false;
    $requete = "INSERT INTO clients (uuid, prenom, nomfamille) VALUES (?, ?, ?)";
    try {
        $stmt = $mysqli->prepare($requete);
        $stmt->bind_param('sss', $uuid, $prenom, $nomfamille);
        $stmt->execute();
        if (0 == $stmt->errno) {
            $retour = true;
        }
        else {
            log_error("$messageErreurAjout - uuid: $uuid - nom: $nomfamille - prenom: $prenom - $stmt->error");
            $tableauRetour['erreurs'][] = ['code' => $codeErreurAjout, 'uuid' => $uuid, 'message' => $messageErreurAjout];
        }
        $stmt->close();
    } catch (Exception $e) {
        log_error("$messageErreurAjout - uuid: $uuid - nom: $nomfamille - prenom: $prenom - $mysqli->error");
        $tableauRetour['erreurs'][] = ['code' => $codeErreurAjout, 'uuid' => $uuid, 'message' => $messageErreurAjout];
    } catch (Error $e) {
        log_error("$messageErreurAjout - uuid: $uuid - nom: $nomfamille - prenom: $prenom - " . $e->getMessage());
        $tableauRetour['erreurs'][] = ['code' => $codeErreurAjout, 'uuid' => $uuid, 'message' => $messageErreurAjout];
    }
    return $retour;
}
/**
 * Supprime un client de la BD distante selon son UUID.
 *
 * @param String $uuid       Identifiant unique universel du client.
 * @param String $nomfamille Nom de famille du client.
 * @param String $prenom     Prénom du client.
 *
 * @author Christiane Lagacé <christianelagace.com>
 *
 * @return bool True si l'opération a réussi.
 *
 */
function suppressionClient($uuid, $nomfamille, $prenom) {
    global $mysqli;
    global $messageErreurSuppression;
    global $codeErreurSuppression;
    global $tableauRetour;
    $retour = false;
    $requete = "DELETE FROM clients WHERE uuid = ?";
    try {
        $stmt = $mysqli->prepare($requete);
        $stmt->bind_param('s', $uuid);
        $stmt->execute();
        if (0 == $stmt->errno) {
            $retour = true;
        }
        else {
            log_error("$messageErreurSuppression - uuid: $uuid - nom: $nomfamille - prenom: $prenom - $stmt->error");
            $tableauRetour['erreurs'][] = ['code' => $codeErreurSuppression, 'uuid' => $uuid, 'message' =>
$messageErreurSuppression];
        }
        $stmt->close();
    } catch (Exception $e) {
        log_error("$messageErreurSuppression - uuid: $uuid - nom: $nomfamille - prenom: $prenom - $mysqli->error");
        $tableauRetour['erreurs'][] = ['code' => $codeErreurSuppression, 'uuid' => $uuid, 'message' =>
$messageErreurSuppression];
    } catch (Error $e) {
        log_error("$messageErreurSuppression - uuid: $uuid - nom: $nomfamille - prenom: $prenom - " . $e->getMessage());
        $tableauRetour['erreurs'][] = ['code' => $codeErreurSuppression, 'uuid' => $uuid, 'message' =>
$messageErreurSuppression];
    }
    return $retour;
}
/**
 * Recherche une valeur dans un tableau d'objets.
 *
 * @param mixed $valeur Valeur recherchée.
 * @param array $tableau Tableau d'objets dans lequel on effectue la recherche.
 * @param string $champ Nom du champ dans lequel on recherche la valeur.
 *
 * @author Christiane Lagacé <christianelagace.com>
 *
 * @return bool True si la valeur a été trouvée.
 *
 */
function presentDansTableauDObjets($valeur, $tableau, $champ) {
    $retour = false;
    foreach($tableau as $objet) {
        if ($objet->$champ == $valeur) {
            $retour = true;
            break;
        }
    }
    return $retour;
}
/**
 * Enregistre la date suivie d'un message d'information dans le fichier journal.
 *
 * Suppositions critiques : Le chemin complet du fichier dont le nom et le chemin sont dans la constante LOG_FILE doit exister
(le fichier sera créé s'il n'existe pas).
 * Les droits sur ce fichier et/ou son dossier doivent permettre au serveur Web de lire et d'écrire dans ce fichier.
 *
 * @param String $message Message à inscrire dans le journal.
 *
 * @author Christiane Lagacé <christianelagace.com>
 *
 */
function log_info($message) {
    if (is_array($message) || is_object($message)) {
        $message = print_r($message, true);
    }
    if (defined('LOG_FILE')) {
        error_log(date("F j, Y, g:i a") . " - Information: $message" . PHP_EOL, 3, LOG_FILE);
    }
    else {
        error_log(date("F j, Y, g:i a") . " - Information: $message". PHP_EOL);
    }
}
/**
 * Enregistre la date suivie d'un message d'erreur dans le fichier journal.
 *
 * Suppositions critiques : Le chemin complet du fichier dont le nom et le chemin sont dans la constante LOG_FILE doit exister
(le fichier sera créé s'il n'existe pas).
 * Les droits sur ce fichier et/ou son dossier doivent permettre au serveur Web de lire et d'écrire dans ce fichier.
 *
 * @param String $message Message à inscrire dans le journal.
 *
 * @author Christiane Lagacé <christianelagace.com>
 *
 */
function log_error($message) {
    if (is_array($message) || is_object($message)) {
        $message = print_r($message, true);
    }
    if (defined('LOG_FILE')) {
        error_log(date("F j, Y, g:i a") . " - Erreur: $message" . PHP_EOL, 3, LOG_FILE);
    }
    else {
        error_log(date("F j, Y, g:i a") . " - Erreur: $message". PHP_EOL);
    }
}
```


### Tester le service Web manuellement


Avant de tenter de consommer le service Web dans une application mobile, il est bon de tester son fonctionnement de façon manuelle.


La technique pour effectuer un tel test est expliquée dans la fiche « **tester_un_service_web_manuellement** ».


### Consommer le service Web


Une fois le service Web écrit et testé, vous êtes prêts à le consommer dans votre application.


#### **avec SwiftUI**


#### **avec Jetpack Compose**


#### Pour plus d'information


* [« How to Test and Play with Web APIs the Easy Way with Postman » - Free Code Camp](https://www.freecodecamp.org/news/how-to-test-and-play-with-web-apis-)
the-easy-way-with-postman


### * [« Debug a PHP HTTP request » - phpStorm](https://www.jetbrains.com/help/phpstorm/debugging-a-php-http-request.html#create_http_request_debug_config)

---