# Extract-Code

Ce dossier contient des extraits de codes développés pendant un stage de Data Analyste dans une start-up de télécommunications. Deux types sont présents :

- Les fichiers présentant un nom commencant par SMS sont des extraits de code SQL permettant le traitement des données SMS de l'entreprise et leur ajout à une table agrégée sur la base de données.
Celle-ci sera lue par un outil de dashboarding afin de pouvoir réaliser différents indicateurs visuels

- Le fichier Script contient le code permettant l'importation, le traitement et l'exportation vers la base de données MySQL du fichier du département Finance, à l'origine sous la forme d'un Excel résultant d'une exécution logicielle


## Plus en détail ##
## SQL ##
SMS_Update_meta :

Les données SMS étant sous la forme d'une table par mois, ajoutée en début du mois M pour les données du mois M-1, il faut automatiser un processus de traitement de ces données dès leur ajout.
Pour cela, une table Meta a été crée. Celle-ci recense les différentes tables présentes sur la base de données SMS, et leur affecte une valeur 'Processsed' (0 ou 1), en fonction de l'intégration ou non des données de cette table sur la table agrégée.
Update_meta permet de mettre à jour cette table en récupérant les noms des tables de la base et en ajoutant les nouvelles tables avec une valeur de Processed à 0.


SMS_Populate_ag :

Cette procédure stockée ("SP") permet, pour les tables référencées dans la table Meta comme ayant une valeur de 'Processed' à 0, d'appliquer les différentes procédures stockées de traitement et ainsi intégrer les données de ces tables aux tables agrégées.
Une mise à jour de la table Meta s'en suit pour signifier la prise en compte de ces données.


SMS_Insert_sms_ag :

Cette procédure stockée permet de grouper les données présentes sur une table SMS d'un mois selon différents critères établis (Client, indicatif téléphonique, statut du SMS etc) et d'insérer cette agrégation dans la table agrégée correspondante.


## Python ##
Script_finance :

Ce script permet le traitement des données du Département Finance, initialement sous la forme d'un Excel ne pouvant être importé directement sur la base de données en raison de son formatage.
L'objectif de ce script est de pouvoir permettre une gestion centralisée sur la base de données, et, dans une optique d'harmonisation des méthodes de visualisation des données de l'entreprise, de pouvoir gérer les indicateurs visuels sur le même outil de dashboarding que les données SMS.
Le script gère l'importation du fichier Excel, la sauvegarde des plages temporelles du document, les sous-totaux correspondants (pour vérification), le retraitement des données, la lecture et l'importation des données de la table "balance_analytique" (résultant du traitement des données financières précédentes), puis l'exportation des nouvelles données vers cette table sur la base.
