# 📱 LAB 18 – ViewModel & LiveData en Android

---

## 📖 Présentation

Ce laboratoire a pour objectif de comprendre et maîtriser les composants d’architecture modernes d’Android :

- ✅ ViewModel
- ✅ LiveData
- ✅ Gestion des changements de configuration (rotation écran)
- ✅ Lifecycle-aware components
<img width="356" height="681" alt="image" src="https://github.com/user-attachments/assets/54fd68ea-cce1-4431-aa5d-a55115b48d37" />
<img width="373" height="728" alt="image" src="https://github.com/user-attachments/assets/74cc78be-0044-4289-aa58-d9004b6c6f52" />

Nous allons comparer :

1. ❌ Version classique (sans ViewModel)
2. ✅ Version moderne (ViewModel + LiveData)

---

# 🎯 Objectifs pédagogiques

À la fin de ce lab, vous serez capable de :

- Comprendre pourquoi une variable d’Activity est perdue à la rotation
- Utiliser `onSaveInstanceState()` et comprendre ses limites
- Implémenter un ViewModel correctement
- Utiliser LiveData pour mettre à jour l’UI automatiquement
- Comprendre le cycle de vie Activity / ViewModel
- Tester les scénarios réels (rotation, thème, process death)

---

# 🧠 Théorie importante

## 🔄 Rotation d’écran

Quand on tourne l’écran :

```
onPause()
onStop()
onDestroy()
onCreate()
```

Android détruit l’Activity et en crée une nouvelle.

👉 Toutes les variables d’instance sont perdues.

---

## ❌ Solution classique : onSaveInstanceState()

```java
@Override
protected void onSaveInstanceState(Bundle outState) {
    outState.putInt("count_key", count);
}
```

✅ Marche pour int, String  
❌ Limité aux types primitifs  
❌ Ne gère pas objets complexes  
❌ Code répétitif  
❌ Ne survit pas au process death  

---

# ✅ ✅ ✅ ViewModel (Solution moderne)

Un ViewModel :

- Survit à la rotation
- Est stocké dans le ViewModelStore
- Ne dépend pas directement de l’UI
- Est détruit seulement si l’Activity est définitivement fermée

---

## 🔥 Exemple :

```java
public class CounterViewModel extends ViewModel {
    private final MutableLiveData<Integer> countLiveData = new MutableLiveData<>(0);
}
```

Même après rotation → valeur conservée ✅

---

# ✅ ✅ ✅ LiveData

LiveData est :

- Observable
- Lifecycle-aware
- Sécurisé
- Évite les memory leaks
- Met à jour l’UI automatiquement

---

## ✅ Observer :

```java
viewModel.getCount().observe(this, new Observer<Integer>() {
    @Override
    public void onChanged(Integer newCount) {
        tvCount.setText(String.valueOf(newCount));
    }
});
```

---

# 🏗 Architecture du projet

```
ViewModelLiveDataDemoEnrichi
│
├── MainActivity.java
├── CounterViewModel.java
│
├── res/layout/activity_main.xml
└── build.gradle
```

---

# 🧪 Tests réalisés

## ✅ Test 1 – Rotation

1. Incrémenter 15 fois
2. Tourner l’écran

✔ Le compteur reste intact

---

## ✅ Test 2 – Mode sombre

Changer thème clair/sombre

✔ Compteur intact

---

## ✅ Test 3 – Process death

```
adb shell am kill com.example.viewmodellivedatademoenrichi
```

✔ Le compteur revient à 0 (normal)
👉 ViewModel ne survit pas à la mort complète du processus.

---

## ✅ Test 4 – Sans LiveData

Commenter l’observe()

✔ L’UI ne se met plus à jour automatiquement

---

# 📊 Comparaison

| Méthode | Rotation | Sécurisé | Recommandé |
|-----------|------------|------------|------------|
| Variable simple | ❌ | ❌ | ❌ |
| onSaveInstanceState | ✅ | ⚠️ | Moyen |
| ViewModel | ✅ | ✅ | ✅ |
| ViewModel + LiveData | ✅ | ✅✅ | ✅✅✅ |

---

# ⚙ Dépendances utilisées

```gradle
implementation "androidx.lifecycle:lifecycle-viewmodel:2.10.0"
implementation "androidx.lifecycle:lifecycle-livedata:2.10.0"
```

Version 2.10.0 compatible Android 15/16.

---

# 🔐 Bonnes pratiques

- Ne jamais stocker logique métier dans l’Activity
- Toujours utiliser ViewModel pour les données UI persistantes
- Utiliser LiveData pour observer proprement
- Éviter les Memory Leak
- Ne pas utiliser onSaveInstanceState pour objets complexes

---

# 🚀 Résultat final

✔ Application stable  
✔ Rotation sans perte de données  
✔ Architecture moderne Android  
✔ Séparation View / Logic  
✔ Lifecycle respecté  

---

# 🧩 Concepts maîtrisés

- LifecycleOwner
- ViewModelStore
- MutableLiveData vs LiveData
- Observer
- setValue()
- Architecture Jetpack

---

# 👨‍💻 Auteur

Projet réalisé dans le cadre du module :

**Programmation Mobile – Android avec Java**

Année universitaire 2025–2026

---

# ✅ Conclusion

Ce laboratoire permet de comprendre un des concepts les plus importants du développement Android moderne :

**La séparation de la logique métier et de l’interface utilisateur.**

ViewModel + LiveData sont aujourd’hui indispensables pour :

- Applications robustes
- Architecture propre (MVVM)
- Gestion correcte du cycle de vie

Ce lab constitue la base de toute application Android professionnelle.
