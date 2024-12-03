![separe](https://github.com/studoo-app/.github/blob/main/profile/studoo-banner-logo.png)
# EVAL 2 - Les triggers SQL Mathias Dabsence
[![Version](https://img.shields.io/badge/Version-1.0.0-blue)]()



- **Nom du Trigger** : `update_dispo_voiture`
- **Événement** : `AFTER INSERT` sur la table `Réservations`
- **Objectif** :
    - Après chaque réservation, le trigger récupère l'id de la voiture réservée et change sa disponibilité à 0 pour indiquer qu'elle n'est plus disponilbe.

#### Code SQL :
```sql
DELIMITER //
CREATE TRIGGER update_dispo_voiture
    AFTER INSERT
    ON Réservations
    FOR EACH ROW
BEGIN
	UPDATE Voitures
    SET disponible = 0
    WHERE id = new.voiture_id;
END;
//
```

- **Nom du Trigger** : `verif_age_client`
- **Événement** : `BEFORE INSERT` sur la table `Réservations`
- **Objectif** :
    - Avant chaque réservation, le trigger vérifie que le client n'a pas moins de 21 ans. Si c'est le cas alors il envoi un message d'erreur précisant qu'il faut avoir 21 ans pour louer une voiture.

#### Code SQL :

```sql
DELIMITER //
CREATE TRIGGER verif_age_client
    BEFORE INSERT
    ON Réservations
    FOR EACH ROW
BEGIN
	IF Clients.age < 21 THEN
    	SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Erreur: Vous devez avoir 21 ans pour louer une voirute';
    END IF;
END;
```


- **Nom du Trigger** : `verif_permis_valide`
- **Événement** : `BEFORE INSERT` sur la table `Clients`
- **Objectif** :
    - Avant d'ajouter un nouveau client, le trigger va vérifier la validité de son permis de conduire.
    - Tout d'abord, si le numéro du permis de conduire ne fait pas 15 caractères, il envoi une erreur disant que celui-ci doit faire 15 caractères.
    - Puis, si le numéro du permis de conduire contient au moins un caractère autre que des chiffres et des lettres alors il envoi une erreur précisant qu'il ne doit contenir que des chiffres et des lettres.
    - Et enfin si le numéro du permis de conduire est vide, il envoi aussi une erreur indiquant qu'il faut posséder un permis de conduire pour s'inscrire.

#### Code SQL :

```sql
DELIMITER //
CREATE TRIGGER verif_permis_valide
    BEFORE INSERT
    ON Clients
    FOR EACH ROW
BEGIN
	IF LENGHT(NEW.permis_conduire) =! 15 THEN
    	SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Erreur: Le numéro de permis de conduire doit comporter 15 caractères';
    ELSEIF NEW.permis_conduire REGEXP '^[^A-Za-z0-9]{1,}$' THEN
    	SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Erreur: Le numéro de permis de conduire ne doit comporter que des chiffres et des lettres';
    ELSEIF NEW.permis_conduire = NULL THEN
    	SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Erreur: Vous devez posséder un permis de conduire pour vous inscrire';
    END IF;
END;
```


- **Nom du Trigger** : `verif_dispo_voiture`
- **Événement** : `BEFORE INSERT` sur la table `Réservations`
- **Objectif** :
    - Avant chaque réservation, le trigger va vérifier que la voiture est bien disponible.
    - On déclare une variable qui va prendre la valeur 0 ou 1 en fonction de la disponibilité de la voiture.
    - Le trigger fait une requête SQL sur la table Voitures pour récupérer la disponibilité de la voiture en fonction de l'id de la réservation et la place dans la variable verif_dispo
    - Si la variable est égal à 0 alors la voiture n'est pas disponible et le trigger envoi un message d'erreur.

#### Code SQL :

```sql
DELIMITER //
CREATE TRIGGER verif_dispo_voiture
    BEFORE INSERT
    ON Réservations
    FOR EACH ROW
BEGIN
	DECLARE verif_dispo INT;

	SELECT disponible into verif_dispo
    FROM Voitures
    WHERE id = NEW.voiture_id;
    
	IF verif_dispo = 0 THEN
    	SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Erreur: La voiture n est pas disponible';
    END IF;
END;
```


- **Nom du Trigger** : `Nom_du_trigger`
- **Événement** : `AFTER INSERT` sur la table `nom_table`
- **Objectif** :
    - Indiquer les actions que doit effectuer le trigger

#### Code SQL :

```sql
CODE SQL DU TRIGGER

```

