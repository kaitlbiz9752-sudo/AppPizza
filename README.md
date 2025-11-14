### Travail réalisée par AITLBIZ Kaoutar
### ENS Marrakech
### Sous la direction de LACHGAR Mohammed

--------------------




#  Projet Android “PizzaRecipes”

## 🍕 Présentation

Ce projet Android a pour objectif de créer une application permettant d’afficher une liste de pizzas accompagnées de leurs images, descriptions, ingrédients et étapes de préparation.
Un Splash Screen élégant apparaît au lancement, puis l’utilisateur accède à la liste des recettes, et enfin à une fiche détaillée pour chaque pizza.



---------------------



##  Objectifs pédagogiques

Ce TP permet de comprendre :

 - Organisation d’un projet Android en packages logiques :

```text classes``` : entités métiers

```text dao``` : interfaces CRUD

```text service``` : gestion des données

```text adapter``` : affichage personnalisé (ListView)

```text ui``` : activités et interfaces utilisateur

- Manipulation d’une ListView avec un Adapter personnalisé


- Passage de données entre activités (Intent + extras)


- Création d’un Splash Screen


- Construction d’un modèle de données orienté objet



------------------



## Architecture du projet


**AFFICHAGE**

<img width="379" height="1013" alt="image" src="https://github.com/user-attachments/assets/0d1b0710-f609-4d6e-91f5-50fd4661da90" />


-----------------



## Étape 1 — Structure du projet

1. Créer un nouveau projet PizzaRecipes :

- Langage : Java

- API minimum : 24

2. Créer les dossiers :


```text
classes/
dao/
service/
adapter/
ui/
```



3. Ajouter dans res/drawable/ les images :



```text
pizza1.jpg
pizza2.jpg
...
pizza10.jpg
```



## Étape 2 — Modèle Produit

**📄 classes/Produit.java**



```text
package com.example.pizzarecipes.classes;

public class Produit {
    private static long AUTO_ID = 1;

    private long id;
    private String nom;
    private double prix;
    private int imageRes;
    private String duree;
    private String ingredients;
    private String description;
    private String etapes;

    public Produit() {
        this.id = AUTO_ID++;
    }

    public Produit(String nom, double prix, int imageRes, String duree,
                   String ingredients, String description, String etapes) {
        this.id = AUTO_ID++;
        this.nom = nom;
        this.prix = prix;
        this.imageRes = imageRes;
        this.duree = duree;
        this.ingredients = ingredients;
        this.description = description;
        this.etapes = etapes;
    }

    public long getId() { return id; }
    public String getNom() { return nom; }
    public double getPrix() { return prix; }
    public int getImageRes() { return imageRes; }
    public String getDuree() { return duree; }
    public String getIngredients() { return ingredients; }
    public String getDescription() { return description; }
    public String getEtapes() { return etapes; }

    @Override
    public String toString() {
        return nom + " - " + prix + " €";
    }
}
```



## Étape 3 — Interface DAO

**📄 dao/IDao.java**



```text
package com.example.pizzarecipes.dao;

import java.util.List;

public interface IDao<T> {
    T create(T t);
    T update(T t);
    boolean delete(long id);
    T findById(long id);
    List<T> findAll();
}
```





-------------------




## Étape 4 — Service ProduitService

Ce service simule une base de données interne.


**AFFICHAGE**

<img width="401" height="822" alt="image" src="https://github.com/user-attachments/assets/66c0556f-6d76-486a-bd06-b3a1f753a66e" />



**📄 service/ProduitService.java**
```text
package com.example.pizzarecipes.service;

import com.example.pizzarecipes.R;
import com.example.pizzarecipes.classes.Produit;
import com.example.pizzarecipes.dao.IDao;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class ProduitService implements IDao<Produit> {

    private static ProduitService INSTANCE;
    private final List<Produit> data = new ArrayList<>();

    private ProduitService() {
        seed(); // préremplissage
    }

    public static ProduitService getInstance() {
        if (INSTANCE == null) INSTANCE = new ProduitService();
        return INSTANCE;
    }

    private void seed() {
        // 1 - BARBECUED CHICKEN PIZZA
        create(new Produit("BARBECUED CHICKEN PIZZA", 3,
                R.drawable.pizza1, "35 min",
                "- 2 boneless skinless chicken breast halves (6 ounces each)\n- 1/4 teaspoon pepper\n- 1 cup barbecue sauce, divided\n- 1 tube (13.8 ounces) refrigerated pizza crust\n- 2 teaspoons olive oil\n-2 cups shredded Gouda cheese\n-1 small red onion, halved and thinly sliced\n-1/4 cup minced fresh cilantro",
                "So fast and so easy with refrigerated pizza crust, these saucy, smoky pizzas make quick fans with their hot-off-the-grill, rustic flavor. They're perfect for spur-of-the-moment cookouts and summer dinners on the patio. —Alicia Trevithick, Temecula, California",
                "STEP 1:\n\n  Sprinkle chicken with pepper; place on an oiled grill rack over medium heat. Grill, covered, until a thermometer reads 165°, 5-7 minutes per side, basting frequently with 1/2 cup barbecue sauce during the last 4 minutes. Cool slightly. Cut into cubes.\n\nSTEP 2:\n\n  Divide dough in half. On a well-greased large sheet of heavy-duty foil, press each portion of dough into a 10x8-in. rectangle; brush lightly with oil. Invert dough onto grill rack; peel off foil. Grill, covered, over medium heat until bottom is lightly browned, 1-2 minutes.\n\nSTEP 3:\n\n  Remove from grill. Spread grilled sides with remaining barbecue sauce. Top with cheese, chicken and onion. Grill, covered, until bottom is lightly browned and cheese is melted, 2-3 minutes. Sprinkle with cilantro. Yield: 2 pizzas (4 pieces each)."));

        // 2 - BRUSCHETTA PIZZA
        create(new Produit("BRUSCHETTA PIZZA ", 5,
                R.drawable.pizza2, "35 min",
                "- 1/2 pound reduced-fat bulk pork sausage\n- 1 prebaked 12-inch pizza crust\n- 1 package (6 ounces) sliced turkey pepperoni\n- 2 cups shredded part-skim mozzarella cheese\n- 1-1/2 cups chopped plum tomatoes\n- 1/2 cup fresh basil leaves, thinly sliced\n- 1 tablespoon olive oil\n- 2 garlic cloves, minced\n- 1/2 teaspoon minced fresh thyme or 1/8 teaspoon dried thyme\n- 1/2 teaspoon balsamic vinegar\n- 1/4 teaspoon salt\n- 1/8 teaspoon pepper\n- Additional fresh basil leaves, optional",
                "You might need a knife and fork for this hearty pizza! Loaded with Italian flavor and plenty of fresh tomatoes, it's bound to become a family favorite. It's even better with a homemade, whole wheat crust! —Debra Kell, Owasso, Oklahoma",
                "STEP 1:\n\n  In a small skillet, cook sausage over medium heat until no longer pink; drain. Place crust on an ungreased baking sheet. Top with pepperoni, sausage and cheese. Bake at 450° for 10-12 minutes or until cheese is melted.\n\nSTEP 2:\n\n  In a small bowl, combine the tomatoes, sliced basil, oil, garlic, thyme, vinegar, salt and pepper. Spoon over pizza. Garnish with additional basil if desired. Yield: 8 slices."));

        // 3 - SPINACH PIZZA
        create(new Produit("SPINACH PIZZA", 2,
                R.drawable.pizza3, "25 min",
                "- 1 package (6-1/2 ounces) pizza crust mix\n- 1/2 cup Alfredo sauce\n- 2 medium tomatoes\n- 4 cups chopped fresh spinach\n- 2 cups shredded Italian cheese blend",
                "This tasty pizza is so easy to prepare. My family, including my young daughter, loves it. What an easy way to make a delicious, veggie-filled meal! —Dawn Bartholomew, Raleigh, North Carolina",
                "STEP 1:\n\n  Prepare pizza dough according to package directions. With floured hands, press dough onto a greased 12-in. pizza pan.\n\nSTEP 2:\n\n  Spread Alfredo sauce over dough to within 1 in. of edges. Thinly slice or chop tomatoes; top pizza with spinach, tomatoes and cheese.\n\nSTEP 3:\n\n  Bake at 450° for 10-15 minutes or until cheese is melted and crust is golden brown. Yield: 4-6 servings."));

        // 4 - DEEP-DISH SAUSAGE PIZZA
        create(new Produit("DEEP-DISH SAUSAGE PIZZA ", 8,
                R.drawable.pizza4, "45 min",
                "- 1 package (1/4 ounce) active dry yeast\n- 2/3 cup warm water (110° to 115°)\n- 1-3/4 to 2 cups all-purpose flour\n- 1/4 cup vegetable oil\n- 1 teaspoon each dried oregano, basil and marjoram\n- 1/2 teaspoon garlic salt\n- 1/2 teaspoon onion salt\n",
                "My Grandma made the tastiest snacks for us when we stayed the night at her farm. Her wonderful pizza, hot from the oven, was covered with cheese and had fragrant herbs in the crust. Now this pizza is frequently a meal for my husband and me and our two young daughters. —Michele Madden, Washington Court House, Ohio",
                "STEP 1:\n\n  In a mixing bowl, dissolve yeast in water. Add 1 cup flour, oil and seasonings; beat until smooth. Add enough remaining flour to form a soft dough. turn onto a floured surface; knead until smooth and elastic, 6-8 minutes. Place in a greased bowl; turn once to greased top. Cover and let rise in a warm place until doubled, about 1 hour. Punch dough down; roll out into a 15-in. circle. Transfer to a well-greased 12-in. heavy ovenproof skillet, letting dough drape over edges. Sprinkle with 1 cup mozzarella.\n\nSTEP 2:\n\n  In another skillet, saute onion, green peppers and seasonings in oil until tender; drain. Layer half of the mixture over crust. Layer with half of the Parmesan, sausage and tomatoes. Sprinkle with 2 cups mozzarella. Repeat layers. Fold crust over to form an edge. Bake for 400° for 20 minutes. Sprinkle with pepperoni and remaining mozzarella. Bake 10-15 minutes longer or until crust is browned. Let stand 10 minutes before slicing. Yield: 8 slices."));

        // 5 - HOMEMADE PIZZA
        create(new Produit("HOMEMADE PIZZA", 4,
                R.drawable.pizza5, "50 min",
                "- 1 package (1/4 ounce) active dry yeast\n- 1 teaspoon sugar\n- 1-1/4 cups warm water (110° to 115°)\n- 1/4 cup canola oil\n- 1 teaspoon salt\n- 3-1/2 cups all-purpose flour\n- 1/2 pound ground beef\n- 1 small onion, chopped\n- 1 can (15 ounces) tomato sauce\n- 1 can (15 ounces) tomato sauce\n- 1 teaspoon dried basil\n- 1 medium green pepper, diced\n- 2 cups shredded part-skim mozzarella cheese",
                "This recipe is a hearty, zesty main dish with a crisp, golden crust. Feel free to use whatever toppings your family enjoys on these homemade pizzas. —Marianne Edwards, Lake Stevens, Washington\n",
                "STEP 1:\n\n  In large bowl, dissolve yeast and sugar in water; let stand for 5 minutes. Add oil and salt. Stir in flour, a cup at a time, until a soft dough forms.\n\nSTEP 2:\n\n  Turn onto floured surface; knead until smooth and elastic, about 2-3 minutes. Place in a greased bowl, turning once to grease the top. Cover and let rise in a warm place until doubled, about 45 minutes. Meanwhile, cook beef and onion over medium heat until no longer pink; drain.\n\nSTEP 3:\n\n  Punch down dough; divide in half. Press each into a greased 12-in. pizza pan. Combine the tomato sauce, oregano and basil; spread over each crust. Top with beef mixture, green pepper and cheese.\n\nSTEP 4:\n\n  Bake at 400° for 25-30 minutes or until crust is lightly browned. Yield: 2 pizzas (3 servings each)."));

        // 6 - PESTO CHICKEN PIZZA
        create(new Produit("PESTO CHICKEN PIZZA", 3,
                R.drawable.pizza6, "50 min",
                "- 2 teaspoons active dry yeast\n- 1 cup warm water (110° to 115°)\n- 2-3/4 cups bread flour\n- 1 tablespoon plus 2 teaspoons olive oil, divided\n- 1 tablespoon sugar\n- 1-1/2 teaspoons salt, divided\n- 1/2 pound boneless skinless chicken breasts, cut into 1/2-inch pieces\n- 1 small onion, halved and thinly sliced\n- 1/2 each small green, sweet red and yellow peppers, julienned\n- 1/2 cup sliced fresh mushrooms\n- 3 tablespoons prepared pesto\n- 1-1/2 cups (6 ounces) shredded part-skim mozzarella cheese\n- 1/4 teaspoon pepper",
                "This is the only pizza I make now. We love it! Keeping the spices simple helps the flavors of the chicken and vegetables come through. The pizza tastes incredible and is good for you, too. —Heather Thompson, Woodland Hills, California",
                "STEP 1:\n\n  In a large bowl, dissolve yeast in warm water. Beat in 1 cup flour, 1 tablespoon oil, sugar and 1 teaspoon salt. Add the remaining flour; beat until combined.\n\nSTEP 2:\n\n  Turn onto a lightly floured surface; knead until smooth and elastic, about 6-8 minutes. Place in a bowl coated with cooking spray, turning once to coat top. Cover and let rise in a warm place until doubled, about 1 hour.\n\nSTEP 3:\n\n  In a large nonstick skillet over medium heat, cook the chicken, onion, peppers and mushrooms in remaining oil until chicken is no longer pink and vegetables are tender. Remove from the heat; set aside.\n\nSTEP 4:\n\n  Punch dough down; roll into a 15-in. circle. Transfer to a 14-in. pizza pan. Build up edges slightly. Spread with pesto. Top with chicken mixture and cheese. Sprinkle with pepper and remaining salt.\n\nSTEP 5:\n\n  Bake at 400° for 18-20 minutes or until crust and cheese are lightly browned. \n\nSTEP 6:\n\n  Freeze option: Bake pizza crust as directed; cool. Top with all the ingredients as directed and securely wrap and freeze unbaked pizza. To use, unwrap pizza; bake as directed, increasing time as necessary. Yield: 8 slices."));

        // 7 - LOADED MEXICAN PIZZA
        create(new Produit("LOADED MEXICAN PIZZA", 3,
                R.drawable.pizza7, "30 min",
                "- 1 can (15 ounces) black beans, rinsed and drained\n- 1 medium red onion, chopped\n- 1 small sweet yellow pepper, chopped\n- 3 teaspoons chili powder\n- 3/4 teaspoon ground cumin\n- 3 medium tomatoes, chopped\n- 1 jalapeno pepper, seeded and finely chopped\n- 1 garlic clove, minced\n- 1 prebaked 12-inch thin pizza crust\n- 2 cups chopped fresh spinach\n- 2 tablespoons minced fresh cilantro\n- Hot pepper sauce to taste\n- 1/2 cup shredded reduced-fat cheddar cheese\n- 1/2 cup shredded pepper jack cheese",
                "My husband is a picky eater, but this healthy pizza has lots of flavor, and he actually looks forward to it. Leftovers are no problem, because this meal tastes better the next day. —Mary Barker, Knoxville, Tennessee",
                "STEP 1:\n\n  In a small bowl, mash black beans. Stir in the onion, yellow pepper, chili powder and cumin. In another bowl, combine the tomatoes, jalapeno and garlic.\n\nSTEP 2:\n\n  Place crust on an ungreased 12-in. pizza pan; spread with bean mixture. Top with tomato mixture and spinach. Sprinkle with cilantro, pepper sauce and cheeses.\n\nSTEP 3:\n\n  Bake at 400° for 12-15 minutes or until cheese is melted. Yield: 6 slices."));

        // 8 - BACON CHEESEBURGER PIZZA
        create(new Produit("BACON CHEESEBURGER PIZZA", 2,
                R.drawable.pizza8, "20 min",
                "- 1/2 pound ground beef\n- 1 small onion, chopped\n- 1 prebaked Italian bread shell crust ( 1pound)\n- 1 can (8 ounces) pizza sauce\n- 6 bacon strips, cooked and crumbled\n- 20 dill pickle coin slices\n- 2 cups (8 ounces) shredded mozzarella cheese\n- 2 cups (8 ounces) shredded cheddar cheese\n- 1 teaspoon pizza or Italian seasoning",
                "Kids of all ages love pizza and cheeseburgers, and this recipe combines them both. My grandchildren usually request pizza for supper when they visit me. They like to help me assemble this version, and they especially enjoy eating it! —Cherie Ackerman, Lakeland, Minnesota",
                "STEP 1:\n\n  In a skillet, cook beef and onion over medium heat until meat is no longer pink; drain and set aside.\n\nSTEP 2:\n\n  Place crust on an ungreased 12-in. pizza pan. Spread with pizza sauce. Top with beef mixture, bacon, pickles and cheeses. Sprinkle with pizza seasoning. Bake at 450° for 8-10 minutes or until cheese is melted. Yield: 8 slices."));


    }

    @Override
    public Produit create(Produit p) {
        data.add(p);
        return p;
    }

    @Override
    public Produit update(Produit p) {
        for (int i = 0; i < data.size(); i++) {
            if (data.get(i).getId() == p.getId()) {
                data.set(i, p);
                return p;
            }
        }
        return null;
    }

    @Override
    public boolean delete(long id) {
        return data.removeIf(x -> x.getId() == id);
    }

    @Override
    public Produit findById(long id) {
        for (Produit p : data) if (p.getId() == id) return p;
        return null;
    }

    @Override
    public List<Produit> findAll() {
        return Collections.unmodifiableList(data);
    }
}
```




**EXEMPLE D'AFFICHAGE**


<img width="402" height="828" alt="image" src="https://github.com/user-attachments/assets/e898a37b-d059-42fe-90b6-179202d55699" />




--------------




## Étape 5 — Splash Screen élégant


1. Layout Splash





**📄 res/layout/activity_splash.xml**



```text
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FBEED8">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:gravity="center"
        android:padding="16dp">

        <!--Logo -->
        <ImageView
            android:id="@+id/imgLogo"
            android:layout_width="220dp"
            android:layout_height="220dp"
            android:src="@drawable/pizza9"
            android:scaleType="fitCenter"/>

        <!-- Texte élégant -->
        <TextView
            android:id="@+id/tvSplashText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="🍕Laissez parler votre cœur… et votre pizza!❤️"
            android:textSize="22sp"
            android:textStyle="italic"
            android:fontFamily="serif"
            android:textColor="#8B2E1F"
            android:letterSpacing="0.05"
            android:gravity="center"
            android:layout_marginTop="24dp"/>
    </LinearLayout>

</FrameLayout>
```


**AFFICHAGE**

<img width="398" height="829" alt="image" src="https://github.com/user-attachments/assets/8f1dfc10-5656-494a-8ff4-175dde37edc9" />




2. Classe Splash

**📄 ui/SplashActivity.java**


```text
package com.example.pizzarecipes.ui;

import android.content.Intent;
import android.os.Bundle;

import androidx.appcompat.app.AppCompatActivity;

import com.example.pizzarecipes.R;

public class SplashActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle b) {
        super.onCreate(b);
        setContentView(R.layout.activity_splash);

        Thread t = new Thread(() -> {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException ignored) {}
            startActivity(new Intent(SplashActivity.this, ListPizzaActivity.class));
            finish();
        });
        t.start();
    }
}
```









----------------------







## Étape 6 — Liste des pizzas

**📄 activity_list_pizza.xml**



```text
<ListView
    android:id="@+id/lvPizzas"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:dividerHeight="6dp"/>
```




**📄 row_pizza.xml**




```text
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:padding="8dp"
    android:layout_width="match_parent"
    android:layout_height="96dp">

    <ImageView
        android:id="@+id/imgPizza"
        android:layout_width="96dp"
        android:layout_height="match_parent"
        android:scaleType="centerCrop"/>

    <TextView
        android:id="@+id/tvNom"
        android:layout_toRightOf="@id/imgPizza"
        android:layout_marginStart="12dp"
        android:textStyle="bold"
        android:textSize="16sp"/>

    <TextView
        android:id="@+id/tvMeta"
        android:layout_below="@id/tvNom"
        android:layout_toRightOf="@id/imgPizza"
        android:layout_marginStart="12dp"/>
</RelativeLayout>
```




----------------





## Étape 7 — Adapter personnalisé

**📄 PizzaAdapter.java**



```text
public class PizzaAdapter extends BaseAdapter {
    private final Context ctx;
    private final List<Produit> pizzas;

    public PizzaAdapter(Context ctx, List<Produit> pizzas) {
        this.ctx = ctx;
        this.pizzas = pizzas;
    }

    @Override public int getCount() { return pizzas.size(); }
    @Override public Object getItem(int i) { return pizzas.get(i); }
    @Override public long getItemId(int i) { return pizzas.get(i).getId(); }

    @Override
    public View getView(int pos, View convertView, ViewGroup parent) {
        if (convertView == null)
            convertView = LayoutInflater.from(ctx).inflate(R.layout.row_pizza, parent, false);

        ImageView img = convertView.findViewById(R.id.imgPizza);
        TextView tvNom = convertView.findViewById(R.id.tvNom);
        TextView tvMeta = convertView.findViewById(R.id.tvMeta);

        Produit p = pizzas.get(pos);

        img.setImageResource(p.getImageRes());
        tvNom.setText(p.getNom());
        tvMeta.setText(p.getDuree() + " • " + p.getPrix() + " €");

        return convertView;
    }
}
```




-----------------





## Étape 8 — Activité Liste

**📄 ui/ListPizzaActivity.java**



```text
public class ListPizzaActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle b) {
        super.onCreate(b);
        setContentView(R.layout.activity_list_pizza);

        ListView lv = findViewById(R.id.lvPizzas);
        List<Produit> pizzas = ProduitService.getInstance().findAll();

        lv.setAdapter(new PizzaAdapter(this, pizzas));

        lv.setOnItemClickListener((parent, view, pos, id) -> {
            Intent it = new Intent(this, PizzaDetailActivity.class);
            it.putExtra("pizza_id", id);
            startActivity(it);
        });
    }
}
```




--------------------




## Étape 9 — Détails d’une pizza

**📄 activity_pizza_detail.xml**





```text
<ScrollView ...>

    <LinearLayout ...>

        <ImageView android:id="@+id/img" ... />

        <TextView android:id="@+id/title" ... />

        <TextView android:id="@+id/meta" ... />

        <TextView android:text="Ingrédients :" ... />
        <TextView android:id="@+id/ingredients" ... />

        <TextView android:text="Description :" ... />
        <TextView android:id="@+id/desc" ... />

        <TextView android:text="Étapes :" ... />
        <TextView android:id="@+id/steps" ... />

    </LinearLayout>

</ScrollView>
```




**📄 PizzaDetailActivity.java**




```text
public class PizzaDetailActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle b) {
        super.onCreate(b);
        setContentView(R.layout.activity_pizza_detail);

        long id = getIntent().getLongExtra("pizza_id", -1);
        Produit p = ProduitService.getInstance().findById(id);

        if (p != null) {
            ((ImageView)findViewById(R.id.img)).setImageResource(p.getImageRes());
            ((TextView)findViewById(R.id.title)).setText(p.getNom());
            ((TextView)findViewById(R.id.meta)).setText(p.getDuree() + " • " + p.getPrix() + " €");
            ((TextView)findViewById(R.id.ingredients)).setText(p.getIngredients());
            ((TextView)findViewById(R.id.desc)).setText(p.getDescription());
            ((TextView)findViewById(R.id.steps)).setText(p.getEtapes());
        }
    }
}
```

----------------------

## Pipeline du Projet – PizzaRecipes



| Étape | Objectif | Fichier(s) | Description |
|-------|----------|------------|-------------|
| **1. Création du projet** | Initialiser l’application Android | – | Projet `PizzaRecipes`, ajout des packages : `classes`, `dao`, `service`, `adapter`, `ui`. |
| **2. Modèle de données** | Définir l’objet Pizza | `classes/Produit.java` | Classe Java contenant : id, nom, prix, image, durée, ingrédients, description, étapes. |
| **3. Interface DAO** | Définir les opérations CRUD | `dao/IDao.java` | Interface générique avec : create(), update(), delete(), findById(), findAll(). |
| **4. Service Produit** | Simuler une base de données interne | `service/ProduitService.java` | Singleton contenant une liste fixe de 10 pizzas + implémentation CRUD. |
| **5. Splash Screen** | Afficher un écran d’accueil animé | `ui/SplashActivity.java`, `activity_splash.xml` | Affiche le logo + texte stylé (Allura) pendant 2 secondes avant d’ouvrir la liste. |
| **6. Liste des pizzas** | Afficher toutes les pizzas | `activity_list_pizza.xml`, `row_pizza.xml` | `ListView` avec layout personnalisé pour chaque pizza (image + nom + durée + prix). |
| **7. Adapter** | Lier données ↔ interface | `adapter/PizzaAdapter.java` | Remplit chaque ligne de la ListView avec les infos du produit. |
| **8. Navigation** | Passer à l’écran détail | `ListPizzaActivity.java` | Envoie `pizza_id` via Intent pour ouvrir la fiche de la pizza choisie. |
| **9. Détails d’une pizza** | Afficher recette complète | `activity_pizza_detail.xml`, `PizzaDetailActivity.java` | Affiche image, nom, durée, prix, ingrédients, description, étapes. |
| **10. Ressources graphiques** | Ajouter les images | `res/drawable/` | Ajout des 10 images `pizza1.jpg` → `pizza10.jpg`. |
| **11. Police élégante** | Style parisien pour le splash | `res/font/allura.ttf` | Police personnalisée pour un texte romantique et élégant. |
| **12. Résultat final** | App complète et fonctionnelle | – | Navigation complète : Splash → Liste → Détails. |




----------------------------




## Pipeline technique (processus interne)


| Action | Traitement | Résultat |
|--------|------------|----------|
| Lancement de l’app | Affiche le Splash 2s | Redirection vers la liste |
| Chargement des pizzas | ProduitService.findAll() | Affiche la liste de 10 pizzas |
| Clic sur une pizza | Intent( pizza_id ) | Ouverture de l’écran détail |
| Affichage détail | ProduitService.findById(id) | Affiche la recette complète |



----------------------------



## Conclusion

Le projet PizzaRecipes illustre la conception complète d’une application Android structurée, moderne et fonctionnelle.
Grâce à une architecture organisée (classes, DAO, service, adapter, UI), l’application sépare clairement les responsabilités et assure une meilleure maintenabilité du code.

Ce TP a permis de mettre en pratique plusieurs notions essentielles du développement Android :

- création et gestion d’un modèle de données orienté objet ;

- mise en place d’un service centralisé simulant une base de données ;

- utilisation d’une ListView et d’un adapter personnalisé ;

- navigation entre activités et passage de données via Intent ;

- création d’un Splash Screen professionnel avec logo et texte stylé ;

- affichage détaillé et structuré du contenu d’un produit.

Au final, l’application propose une expérience fluide :

- un écran d’accueil → une liste de pizzas → une fiche détaillée riche en informations.




## Démonstration Vidéo :





https://github.com/user-attachments/assets/a9c53859-5904-4521-8132-64fffdde2a6e



