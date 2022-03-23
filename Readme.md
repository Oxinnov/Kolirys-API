# KOLIRYS.FR, LOGICIEL DE FACTURATION - UTILISATION DE L'API

La connexion à l'API se fait grâce aux deux jetons (Token) privés accessible dans votre espace membre.
* Vos clés API ne s'afficheront que si vous êtes abonné à l'offre PRO

## Procédure de récupération du Token

Rendez-vous sur votre compte Kolirys.fr, connectez-vous et cliquez sur "Mon Compte", puis "API".

## Usage cURL

```php
<?php

// CONFIGURATION
    $TOKEN_USER = /* INSERT YOUR TOKEN_USER HERE */;
    $TOKEN_SECRET = /* INSERT YOUR TOKEN_SECRET HERE */;
    $COMMAND = "Commande";
    $OPTIONS = array();
// END CONFIGURATION

$curl = curl_init();
curl_setopt_array($curl, array(
    CURLOPT_URL => "https://www.kolirys.fr/api/$TOKEN_USER/$TOKEN_SECRET/$COMMAND",
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => "",
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 30,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => "POST",
    CURLOPT_POST => true,
    CURLOPT_POSTFIELDS => json_encode(['options' => json_encode($OPTIONS)]),
    CURLOPT_SSL_VERIFYHOST => 0,
    CURLOPT_SSL_VERIFYPEER => 0,
    CURLOPT_HTTPHEADER => array('Content-Type: application/json')
));
$response = curl_exec($curl);
$err = curl_error($curl);
curl_close($curl);

if ($err) { return "cURL Error #:" . $err; } else { $objectResponse = json_decode($response);
    if ($objectResponse->Success) { return "LOG SUCCESS"; }
    else { return $objectResponse->ErrorMessage; }
}
?>
```

## Liste des commandes

### GenerateDocument

Génère un document (Devis, Facture ou Avoir) avec toutes les informations (sans le valider).

```php
<?php $OPTIONS = [
    'typeDocument' => 'INVOICE', // TYPE DU DOCUMENT (Obligatoire) | "INVOICE" : FACTURE , "QUOTATION" : DEVIS", "CREDIT" : AVOIR
    'numberDocument' => 'F3478', // NUMERO DU DOCUMENT (Facultatif)
    'customText1' => '...', // TEXTE PERSONNALISÉ 1 (Facultatif)
    'customText2' => '...', // TEXTE PERSONNALISÉ 2 (Facultatif)
    'lines' => [
        [
            'REFERENCE' => '90248',                 // REFERENCE (Facultatif)
            'TITLE' => "Main d'oeuvre peinture",    // DESCRIPTION / LIBELLE (Facultatif)
            'PRICE_UNIT' => 34.95,                  // PRIX UNITAIRE (Facultatif)
            'QUANTITY' => 12,                       // QUANTITÉ (Facultatif)
            'DISCOUNT' => 10,                       // REMISE (%) (Facultatif)
            'VAT' => 20,                            // TVA (%) (Facultatif)
            'TYPE' => 1                             // 1 => PRESTATION DE SERVICE | 2 => VENTE DE MARCHANDISE (Facultatif)
        ],
        [
            'TITLE' => 'Pot de peinture 5 Litres',
            'PRICE_UNIT' => 49.99,
            'QUANTITY' => 3,
            'TYPE' => 2,
            'VAT' => 20,
            'REFERENCE' => 'PEINT-NOIR-039'
        ],
        [
            'TITLE' => 'Ligne vide'
        ]
    ],
    'customerID' => 23    // ID DU CLIENT (Facultatif)
]; ?>	
```

#### Retour de l'API

```json
{
    Success : true,
    ErrorMessage : null,
    Data : [
        UrlDocument : @String
        ID : @int
    ]
}
```

### CreateCustomer

Création d'un fichier client

```php
<?php $OPTIONS = array(
'firstName' => 'PRENOM',            /* OBLIGATOIRE POUR LES CLIENTS PARTICULIERS */
'lastName' => 'NOM',                /* OBLIGATOIRE POUR LES CLIENTS PARTICULIERS */
'companyName' => 'NOM ENTREPRISE',  /* OBLIGATOIRE POUR LES CLIENTS PROFESSIONNELS */
'companySiret' => 'NUMERO SIRET',   /* OBLIGATOIRE POUR LES CLIENTS PROFESSIONNELS */
'mail' => 'ADRESSE E-MAIL',         /* FACULTATIF */
'phone' => 'TELEPHONE',             /* FACULTATIF */
'adress' => 'ADRESSE',              /* FACULTATIF */
'zip' => 'CODE POSTAL',             /* FACULTATIF */
'city' => 'VILLE',                  /* FACULTATIF */
'country' => 'PAYS',                /* FACULTATIF */
'typeCustomer' => 1,                  /* 1 => CLIENT PARTICULIER | 2 => CLIENT PROFESSIONNEL */
'tvaNumber' => 'FR20984982492'
); ?>	
```

#### Retour de l'API

```json
{
    Success : true,
    ErrorMessage : null,
    Data : [
        ID_Customer : @int (ID du client)
]}
```

### DeleteCustomer

Supprime un client grâce à son ID (Identifiant unique).	

```php
<?php $OPTIONS = /* ID DU CLIENT */ ?>	
```

#### Retour de l'API

```json
{
    Success : true,
    ErrorMessage : null,
    Data : null
}
```

### GetDocumentPDF

Récupère l'URL du fichier PDF associé à un document.

```php
<?php $OPTIONS = /* ID DU DOCUMENT */ ?>	
```

#### Retour de l'API

```json
{
    Success : true,
    ErrorMessage : null,
    Data : [
        URL : @string
]}
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## Liens utiles
[Kolirys.fr](https://www.kolirys.fr)

[Oxinnov - Agence Web](https://www.oxinnov.fr)

[Documentation API](https://www.kolirys.fr/documentation/api)