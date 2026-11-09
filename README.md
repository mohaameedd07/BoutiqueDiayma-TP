# TP - C# et .NET : Visual Studio et Débogage
Projet : Boutique Diayma
Nom : (mets ton nom ici)

## Récupération du projet
J'ai cloné le projet depuis le dépôt donné par le prof, puis je l'ai ouvert
dans Visual Studio avec le fichier Diayma.sln. J'ai fermé la fenêtre
d'avertissement, et j'ai lancé avec F5.

## Question 2 : les projets de la solution
La solution contient un seul projet : Diayma (fichier Diayma.csproj).
C'est une application web ASP.NET Core MVC.

## Question 3 : version du SDK .NET
Le projet utilise .NET Core 2.0 (netcoreapp2.0), avec le package
Microsoft.AspNetCore.All en version 2.0.6.
On peut le voir dans le fichier Diayma.csproj :
<TargetFramework>netcoreapp2.0</TargetFramework>

## Question 6 : 2 bugs trouvés
Bug 1 : le total du panier est faux.
Dans le fichier Models/Cart.cs, la méthode GetTotalValue() additionne
seulement le prix du produit sans le multiplier par la quantité.
Donc si on met 2 fois le même produit, le total n'augmente pas comme il faut.
Il faut mettre :
return GetCartLineList().Sum(x => x.Product.Price * x.Quantity);

Bug 2 : la valeur moyenne du panier est aussi fausse.
Toujours dans Cart.cs, la méthode GetAverageValue() ne tient pas compte
des quantités non plus. La vraie moyenne devrait être le total divisé par
le nombre total d'articles.

## Question 7 : les points d'arrêt
J'ai placé les points d'arrêt demandés sur ces lignes :
- CartSummaryViewComponent.cs, ligne 12 : _cart = cart as Cart;
- ProductController.cs, ligne 15 : _productService = productService;
- OrderController.cs, ligne 17 : _cart = pCart;
- CartController.cs, ligne 15 : _cart = pCart;
- Startup.cs, ligne 20 : Configuration = configuration;

Toutes ces lignes sont dans des constructeurs.

## Question 8 : namespaces, classes et méthodes visités avant l'affichage des produits
La page d'accueil utilise la route Product/Index. En suivant le code au
débogueur, voici l'ordre que j'ai observé :

Au démarrage :
- Program.Main (namespace P2FixAnAppDotNetCode)
- Startup (constructeur, puis ConfigureServices et Configure)

Quand on charge la page d'accueil :
- ProductController (constructeur) - namespace P2FixAnAppDotNetCode.Controllers
- ProductController.Index()
- ProductService.GetAllProducts() - namespace P2FixAnAppDotNetCode.Models.Services
- ProductRepository.GetAllProducts() - namespace P2FixAnAppDotNetCode.Models.Repositories
(c'est là que la liste des produits est récupérée)
- ensuite la vue Product/Index.cshtml s'affiche
- et le composant CartSummaryViewComponent (namespace P2FixAnAppDotNetCode.Components) s'exécute pour le panier en haut

Mode utilisé :
J'ai utilisé le mode "Pas à pas détaillé" (Step Into, F11), parce que c'est
le seul qui entre à l'intérieur des méthodes et qui permet de voir tous les
namespaces, classes et méthodes traversés.
Le "Pas à pas principal" (F10) passe par-dessus les méthodes sans y entrer,
et le "Pas à pas sortant" (Shift+F11) sert à ressortir d'une méthode.
Donc le "Pas à pas détaillé" est le bon choix pour cette question.

## Question 9 : déploiement en exécutable Windows
J'ai publié l'application en exécutable autonome avec la commande :
dotnet publish -c Release -r win-x64 --self-contained true

L'exécutable se trouve dans le dossier publish, il s'appelle
P2FixAnAppDotNetCode.exe.

## Question 10 : lien vers l'exécutable
J'ai mis l'exécutable (en zip) sur Google Drive :
https://drive.google.com/file/d/1Ksp_cOuUpDj8YPLd7vqMB3y86W35h00A/view?usp=sharing
