# Data Analysis in Earth Sciences

# Analiza Danych Wyjazdów Karetek Pogotowia

Ten projekt zawiera kod Python w notatniku Jupyter (`analizy.ipynb`) służący do dogłębnej analizy danych związanych z wyjazdami karetek pogotowia w województwie małopolskim. Celem analizy jest odkrycie kluczowych wzorców czasowych, przestrzennych oraz routingu, co może wesprzeć optymalizację działania służb ratunkowych, poprawę czasu reakcji i efektywniejsze zarządzanie zasobami.

## Cel Projektu

Główne cele projektu to:
-   **Identyfikacja szczytów zapotrzebowania:** Określenie, kiedy (godzina, dzień tygodnia, miesiąc) występuje największa liczba wezwań/wyjazdów karetek.
-   **Mapowanie obszarów wysokiego ryzyka/zapotrzebowania:** Wizualizacja geograficznych "gorących punktów", z których pochodzi najwięcej wezwań lub do których karetki najczęściej dojeżdżają.

## Analiza Danych

Notatnik `analizy.ipynb` wykorzystuje zaawansowane biblioteki do wizualizacji i analizy danych geoprzestrzennych i czasowych. Dane wejściowe są oczekiwane w formacie `geopandas.GeoDataFrame` (np. z pliku `wynik_caly.parquet`), zawierające kolumny takie jak `start_point` (punkt początkowy wyjazdu karetki/miejsce wezwania), `end_point` (punkt końcowy wyjazdu/miejsce dostarczenia pacjenta), `start_time` (czas rozpoczęcia wyjazdu).

### 1. Analiza Czasowa (`analyze_temporal_patterns`)

Ta sekcja skupia się na wzorcach aktywności karetek w zależności od upływu czasu. Funkcja `analyze_temporal_patterns` wzbogaca dane o kolumny `hour` (godzina), `day_of_week` (dzień tygodnia) i `month` (miesiąc) na podstawie `start_time`.

**Generowane wizualizacje (przy użyciu `matplotlib` i `seaborn`):**
-   **"Liczba wyjazdów w zależności od godziny"**![image](https://github.com/user-attachments/assets/6fd4f55c-c9ac-4f7b-b568-f8f2cb3b323c)
 Wykres słupkowy prezentujący rozkład liczby interwencji w każdej godzinie doby. Pozwala to na łatwe zidentyfikowanie godzin szczytu zapotrzebowania (np. poranny i popołudniowy). Na załączonym wykresie widać wyraźne piki aktywności w godzinach porannych (ok. 6-9) oraz popołudniowych/wieczornych (ok. 16-20).
-   **"Liczba wyjazdów w zależności od dnia tygodnia"** (np. `image_aa0b32.png`): Wykres słupkowy pokazujący zagęszczenie wyjazdów dla każdego dnia tygodnia. Ujawnia różnice w obciążeniu służb między dniami roboczymi a weekendami. Przykładowy wykres pokazuje, że liczba wyjazdów utrzymuje się na podobnym, wysokim poziomie w dni robocze (1-5), z lekkim spadkiem w weekend (0-6).
-   **"Liczba wyjazdów w zależności od miesiąca"** (np. `image_aa0af3.png`): Wykres słupkowy obrazujący zmienność liczby wyjazdów w poszczególnych miesiącach roku, co pozwala na identyfikację trendów sezonowych w zapotrzebowaniu na pomoc medyczną. Na przedstawionym wykresie widać, że najwyższa liczba wyjazdów przypada na początek i koniec roku (np. styczeń, grudzień), podczas gdy miesiące letnie (np. lipiec, sierpień) mogą charakteryzować się nieco niższą aktywnością.

**Przykładowe wnioski wizualne:**
-   Piki wezwań w godzinach szczytu komunikacyjnego i wieczorem mogą sugerować zwiększoną liczbę wypadków lub nagłych zachorowań w tych okresach.
-   Różnice w obciążeniu w poszczególnych dniach tygodnia (np. wzrost w weekendy lub spadek w dni świąteczne) wskazują na potrzebę elastycznego planowania dyżurów.
-   Sezonowe wahania (np. wzrost w zimie związany z infekcjami, spadek w lato z wakacjami) mogą wpływać na zapotrzebowanie na personel i sprzęt.

### 2. Analiza Przestrzenna (`analyze_spatial_patterns`)

Ta część analizy koncentruje się na geograficznym rozkładzie wezwań i miejsc, do których karetki dojeżdżają.

**Generowane wizualizacje (interaktywne mapy `folium` z wtyczkami oraz wykresy gęstości z `contextily`):**
-   **Mapa Ciepła (HeatMap) dla Punktów Początkowych (miejsc wezwań)** (np. `image_aa0eb2.png`): Interaktywna mapa Folium z nałożoną warstwą HeatMap, która wizualizuje gęstość miejsc, z których karetki rozpoczynają wyjazdy (miejsca wezwań/zdarzeń). Ciemniejsze obszary wskazują na miejsca o największej koncentracji zdarzeń. Na załączonej mapie wyraźnie widać skupiska wezwań w centrum miasta oraz w gęsto zaludnionych dzielnicach.
-   **Mapa Ciepła (HeatMap) dla Punktów Końcowych (miejsc docelowych)** (np. `image_aa0ed5.png`): Analogicznie, HeatMap dla punktów końcowych, pokazująca, gdzie pacjenci najczęściej są dostarczani (np. szpitale, placówki medyczne). Na tej mapie skupiska wskazują na lokalizacje głównych szpitali i oddziałów ratunkowych.
-   **Mapa Klastrów Znaczników (MarkerCluster) dla Punktów Początkowych** (np. `image_aa0ef6.png`): Interaktywna mapa z klastrami znaczników, które grupują punkty początkowe. Pozwala to na eksplorację obszarów o wysokiej koncentracji wezwań na różnych poziomach powiększenia.
-   **Mapa Klastrów Znaczników (MarkerCluster) dla Punktów Końcowych** (np. `image_aa0f2e.png`): Klastry znaczników dla punktów końcowych, ułatwiające przeglądanie miejsc docelowych.
-   **Wykresy gęstości 2D (KDE Plot) nałożone na mapę (z `contextily`):**
    -   **"Gęstość punktów początkowych wyjazdów karetek"** (np. `image_aa0735.png`): Wizualizacja gęstości punktów startowych, nałożona na podkład mapowy. Używa `seaborn.kdeplot` do pokazania obszarów o największej koncentracji wezwań, co daje intuicyjny obraz "gorących stref" zapotrzebowania. Widoczne są wyraźne "gorące punkty" w centrum miasta.
    -   **"Gęstość punktów końcowych wyjazdów karetek"** (np. `image_aa07d2.png`): Analogiczny wykres gęstości dla punktów końcowych, wskazujący miejsca, gdzie pacjenci są najczęściej dowożeni. Skupiska te korelują z lokalizacjami szpitali.

**Przykładowe wnioski wizualne:**
-   Heatmapy i wykresy gęstości jasno identyfikują dzielnice lub obszary o największym zapotrzebowaniu na interwencje karetek, co jest kluczowe dla optymalnego rozmieszczenia baz pogotowia.
-   Porównanie map punktów początkowych i końcowych może ujawnić główne placówki medyczne, które obsługują dany obszar, oraz wskazać na główne kierunki transportu pacjentów.
-   Identyfikacja obszarów, które generują dużą liczbę wezwań, jest kluczowa dla strategicznego rozmieszczenia karetek i skrócenia czasu dojazdu.

### 3. Analiza Wzorców Tras (`analyze_route_patterns`)

Ta sekcja ma na celu analizę rzeczywistych połączeń (tras) między miejscami wezwań a punktami docelowymi (np. szpitalami). Wykorzystuje bibliotekę `networkx` do modelowania tych relacji jako grafu, co pozwala na identyfikację kluczowych korytarzy komunikacyjnych.

**Generowane wizualizacje:**
-   **Mapa Tras (PolyLine na Folium):** Interaktywna mapa Folium, na której rysowane są linie reprezentujące trasy wyjazdów karetek. Umożliwia to wizualizację najczęściej wykorzystywanych korytarzy transportowych oraz obszarów o największym obciążeniu ruchem karetek.
-   **Analiza centralności w grafie (networkx):** Obliczenia takie jak `degree_centrality` (stopień centralności) i `betweenness_centrality` (pośrednictwo centralności) mogą pomóc w identyfikacji kluczowych węzłów (obszarów lub punktów) w sieci wyjazdów karetek. Wyniki mogą być wizualizowane na mapie poprzez kolorowanie lub rozmiar znaczników, wskazując na miejsca, które są strategiczne dla przepływu karetek.

**Przykładowe wnioski wizualne:**
-   Wizualizacja tras może pokazać, które drogi lub arterie są najbardziej obciążone przez karetki, co jest ważne dla planowania utrzymania dróg i zarządzania ruchem w sytuacjach awaryjnych.
-   Analiza centralności może wskazać obszary lub skrzyżowania, które są krytyczne dla efektywnego dotarcia do różnych części miasta/regionu, co może sugerować potrzebę poprawy infrastruktury w tych punktach.

## Wykorzystane Technologie

-   **Python 3.x**
-   **Jupyter Notebook**
-   **Biblioteki Python:**
    -   `pandas` i `numpy` do manipulacji i analizy danych tabelarycznych.
    -   `geopandas` do pracy z danymi geoprzestrzennymi (`GeoDataFrame`).
    -   `matplotlib.pyplot` i `seaborn` do tworzenia statycznych i estetycznych wykresów.
    -   `folium` i `folium.plugins` (HeatMap, MarkerCluster) do tworzenia interaktywnych map HTML.
    -   `contextily` do dodawania podkładów mapowych (np. OpenStreetMap) do wykresów geoprzestrzennych.
    -   `shapely.geometry` do tworzenia obiektów geometrycznych (np. punktów, linii).
    -   `networkx` do tworzenia i analizy grafów, co jest kluczowe w analizie wzorców tras.
    -   `datetime` do operacji na datach i czasie.

## Jak uruchomić?

1.  **Sklonuj/Pobierz repozytorium.**
2.  **Zainstaluj wymagane biblioteki:**
    ```bash
    pip install pandas numpy matplotlib seaborn folium contextily geopandas shapely networkx
    ```
    (Jeśli masz plik `requirements.txt`, użyj `pip install -r requirements.txt`).
3.  **Przygotuj dane:** Upewnij się, że plik `wynik_caly.parquet` (lub inny plik z danymi w odpowiednim formacie) jest dostępny w katalogu, z którego uruchamiasz notatnik, lub zmodyfikuj ścieżkę w kodzie. Oczekuje się, że dane będą w formacie `geopandas.GeoDataFrame` zawierającym kolumny geoprzestrzenne oraz kolumnę `start_time` (timestamp).
4.  **Uruchom Jupyter Notebook:**
    ```bash
    jupyter notebook
    ```
5.  **Otwórz `analizy.ipynb`** w przeglądarce i wykonaj poszczególne komórki, aby zobaczyć wyniki analiz i generowane wykresy.

---
Mam nadzieję, że to jest to, czego potrzebujesz!


## Getting started
### Create virtual environment

Make sure you have `conda-lock` installed. If you already have it, run the command below to create the environment based on `conda-lock.yml` file.

```
conda-lock install --mamba -n daes-env conda-lock.yml
```

### Activate environment

```
mamba activate daes-env
```

