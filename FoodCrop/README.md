# FoodCropsPython : Rapport de Projet



<h1>Membres de l'équipe :</h1>

<ul>
   <li>Mohammed EL YOUSFI ALAOUI</li>  
   <li>Chayma TLEMCANI</li>  
   <li>Chaimae MOUMNI</li>  
   <li>Blaise ROUGE </li> 
</ul>


<h1>1 - Description de l'environnement </h1>

Nous avons travaillé principalement dans un environnement Windows cependant un des membres de l'équipe avait un linux ,un autre un mac . Mais cela n'a pas agit sur la collaboration entre nous puisqu'on a travaillé avec la même version de Python qui est la version 3.8.10

lien su git : https://gitlab.mines-ales.fr/chaimae/foodcropspython 



<h1>2 - Instruction pour lancer le programme </h1>

Il faut d'abord se placer dans le repertoire contenant l'ensemble du projet depuis un terminal puis exécuter la commande suivante :  
" python foodCropsDataset.py " ,
Puisque la fonction main qui permet d'exécuter le programme se trouve dans le fichier foodCropsDataset.py
   
    if __name__ == "__main__" :

    print("Bienvenu dans le programme principal \n")
    mi=FoodCropsFactory()
    mi.loadData('FeedGrains.csv')
    """for m in all:
        print(m.describe())"""
    q=int(input("tapez 1 pour continuer ou 0 pour quitter\n"))
    while(q!=0):
        print("Vous allez commencer une recherche \n veuillez choisir vos critères")
        i=0

<h1>3 - Description de la répartition des tâches</h1>

On a travaillé ensemble sur toutes les tâches mais spécifiquement, il y avait certaines personnes qui étaient plus focalisé sur certaines tâches que d'autres.

Par exemple : 

<b>O1: </b>Mohammed EL YOUSFI ALAOUI - Chayma TLEMCANI - Chaimae MOUMNI - Blaise ROUGE </br>
<b>O2: </b>Mohammed EL YOUSFI ALAOUI - Chayma TLEMCANI - Chaimae MOUMNI </br>
<b>O3: </b>Mohammed EL YOUSFI ALAOUI - Chayma TLEMCANI - Chaimae MOUMNI </br>
<b>O4: </b>Mohammed EL YOUSFI ALAOUI - Chayma TLEMCANI - Chaimae MOUMNI </br>
<b>Rapport:</b> Chaimae MOUMNI - Mohammed EL YOUSFI ALAOUI - Chayma TLEMCANI </br>

<h1>4 - Analyse des problèmes rencontrés</h1>

<b>1er problème :</b> Pour les enumérations excel a plus de valeur que l'UML

<b>-->Solution : </b>Pour les deux classes IndicatorGroup et CommodityGroup  tous ce qui a de plus on lui a affecté à OTHER

<b>2ème Problème : </b>Un problème au niveau de l'UML pour les classes Volume, Surface, Count, Price, Weight, Ratio le name est prédifini, si on garde cette structure on aurait pas pris en considération les valeurs des unités excel comme les noms acres ,hectares pour la surface et la meme chose pour les autres (Volume, Count, Price, Weight, Ratio) 

<b>-->Solution :</b> Donner comme arguments pour les fonctions create le parametre name  par exemple pour la fonction  createRatio
    
     def createRatio(self, id: int, name : str) -> Unit:
        if(id not in self.__unitsRegistry):
            ratio = Ratio(id,name)
            self.__unitsRegistry[id] = ratio
        return self.__unitsRegistry[id]


<b>3ème Problème :</b>L'indicateur a plus qu'une unité ce qui créé un probleme au niveau de la recherche par unité

<b>-->Solution :</b> On utilise idi qui est concatenation de 2 id l'un du groupe l'autre de l'unité pour obtenir une clé permettant
             de resoudre le probleme qui fait que les mesures sont cherchés en se basant uniquement sur l'id du groupe 

            #on utilise ici le try catch pour gerer les valeurs autres que celles qu'on a dans l'enumeration 

            # Creation des indicateurs
            try:
                indicatorGroup = IndicatorGroup(sCGroupID)
            except ValueError: 
                indicatorGroup = IndicatorGroup.OTHER 
            idi=str(sCGroupID)+","+str(unit.id)  
            indicator=self.foodCropFactory.createIndicator(idi,sCFrequencyID,sCFrequencyDesc,sCGeographyIndentedDesc,indicatorGroup,unit)

            # Au niveau de createIndicator on récupère l'id du groupe

             def createIndicator(self,id : str, frequency : int , freqDesc : str, geoLocation : str, indicatorGroup : IndicatorGroup, unit : Unit) -> Indicator:
                if(id not in self.__commodityRegistry):
                    idi = id.split(",")[0]
                    indicator = Indicator(idi, frequency, freqDesc, geoLocation, indicatorGroup, unit)
                    self.__indicatorsRegistry[id]=indicator
                return self.__indicatorsRegistry[id]

<b>4ème problème : </b>Lors du loading de l'excel certaines valeurs NaN nous causais un problème pour les caster 

<b>-->Solution :</b> On utilise la fonction fillna(0) sur dataframe pour remplacer les cases vides par un 0 

<b>5ème problème :</b> Pour remplir le multiplier on a un problème au niveau de la valeur  1,000 soit consideré comme 1  une fois casté à cause de la virgule, et aussi de la la valeur de million qui est sous forme d'une chaine de caractères.

<b>-->Solution : </b>On recherche par des expressions régulières (1,000) et (million)et on lui affectent des valeurs de multiplier 

            elif(re.search(r"bushel|ton",unite)):
                multiplier = 1
                if(re.search(r"1,000",unite)):
                    multiplier = 1000
                if(re.search(r"million",unite)):
                    multiplier = 1000000
                unit=self.foodCropFactory.createWeight(sCUnitID,multiplier,sCUnitDesc)




