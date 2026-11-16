
# Exercice 1

- Question 1

 
  
- Question 2

@startuml
Actor Personne
Actor Administrateur
participant ControleurAcces
participant Saisie
participant VoyantR
participant VoyantV
participant Minuterie
participant BD
participant Archive
participant Porte
participant Alarme
Activate ControleurAcces
ControleurAcces --> VoyantR : active
Activate VoyantR
Personne<--ControleurAcces : Demande saisie
Activate Saisie
Activate Personne
Personne-->Saisie : entre chiffre 1
Saisie-->ControleurAcces : envoie chiffre 1
ControleurAcces --> VoyantR : desactive
Deactivate VoyantR
ControleurAcces --> VoyantV : active
Activate VoyantV
ControleurAcces --> Minuterie : lance (5s)
Activate Minuterie
Personne-->Saisie : entre chiffre 2
Saisie-->ControleurAcces : envoie chiffre 2
Personne-->Saisie : entre chiffre 3
Saisie-->ControleurAcces : envoie chiffre 3
Personne-->Saisie : entre chiffre 4
Saisie-->ControleurAcces : envoie code
Deactivate Saisie

ControleurAcces --> Minuterie : arrête
Minuterie ---> ControleurAcces : desactive minuteur
Deactivate Minuterie
ControleurAcces --> Archive : Envoie la tentative
ControleurAcces-->BD : Verifie entree
Activate BD
BD --> ControleurAcces : envoie reponse
Deactivate BD

alt succes 

   ControleurAcces --> Porte : Debloque
   Activate Porte
   ControleurAcces --> Minuterie : lance (20s)
   Activate Minuterie
   Personne --> Porte : ouvre

   ControleurAcces --> Minuterie : arrête
   Minuterie ---> ControleurAcces : desactive minuteur
   Deactivate Minuterie

   alt porte fermee après passage

      ControleurAcces --> Porte : Bloque
      Deactivate Porte

   else porte non fermee après passage

      ControleurAcces --> Alarme : active (temps inconnu)
      Activate Alarme
      Deactivate ControleurAcces
      Activate Administrateur
      Administrateur --> ControleurAcces : Reactive
      Activate ControleurAcces
      Administrateur --> Alarme : Desactive
      Deactivate Alarme
      Deactivate Administrateur

   end
end
ControleurAcces --> VoyantV : desactive
Deactivate VoyantV
ControleurAcces --> VoyantR : active
Activate VoyantR
@enduml
