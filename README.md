# Data Analysis in Earth Sciences

# Analiza Danych Wyjazdów Karetek Pogotowia

Ten projekt zawiera kod Python w notatniku Jupyter (`analizy.ipynb`) służący do dogłębnej analizy danych związanych z wyjazdami karetek pogotowia w województwie małopolskim. Celem analizy jest odkrycie kluczowych wzorców czasowych, przestrzennych oraz routingu, co może wesprzeć optymalizację działania służb ratunkowych, poprawę czasu reakcji i efektywniejsze zarządzanie zasobami.

## Cel Projektu

Główne cele projektu to:
-   **Identyfikacja szczytów zapotrzebowania:** Określenie, kiedy (godzina, dzień tygodnia, miesiąc) występuje największa liczba wezwań/wyjazdów karetek.
-   **Mapowanie obszarów wysokiego ryzyka/zapotrzebowania:** Wizualizacja geograficznych "gorących punktów", z których pochodzi najwięcej wezwań lub do których karetki najczęściej dojeżdżają.
-    **Optymalizacja rozmieszczenia karetek:** Dostarczenie danych wspomagających decyzje o strategicznym rozmieszczeniu baz karetek lub punktów ich postoju w celu skrócenia czasu dojazdu.
-   **Planowanie zasobów ludzkich i sprzętowych:** Lepsze planowanie dyżurów i wyposażenia w zależności od przewidywanego popytu.


## Analiza Danych

Notatnik `analizy.ipynb` wykorzystuje zaawansowane biblioteki do wizualizacji i analizy danych geoprzestrzennych i czasowych. Dane wejściowe przetworzone zostały z formatu .csv do `geopandas.GeoDataFrame`. 

### 1. Analiza Czasowa 

Ta sekcja skupia się na wzorcach aktywności karetek w zależności od upływu czasu. Funkcja `analyze_temporal_patterns` wzbogaca dane o kolumny `hour` (godzina), `day_of_week` (dzień tygodnia) i `month` (miesiąc) na podstawie `start_time`.

**Generowane wizualizacje (przy użyciu `matplotlib` i `seaborn`):**
-   **Liczba wyjazdów w zależności od godziny**![image](https://github.com/user-attachments/assets/6fd4f55c-c9ac-4f7b-b568-f8f2cb3b323c)
    
 Wykres słupkowy prezentujący rozkład liczby interwencji w każdej godzinie doby. Pozwala to na łatwe zidentyfikowanie godzin szczytu zapotrzebowania (np. poranny i popołudniowy). Na załączonym wykresie widać wyraźne piki aktywności w godzinach porannych (ok. 10-11) oraz popołudniowych/wieczornych (ok. 19).
-   **Liczba wyjazdów w zależności od dnia tygodnia** ![image](https://github.com/user-attachments/assets/865339a9-bc7c-4298-a66c-4793ddc7e637)
  
 Wykres słupkowy pokazujący zagęszczenie wyjazdów dla każdego dnia tygodnia. Ujawnia minimalną różnice w obciążeniu służb między dniami roboczymi a weekendami. Przykładowy wykres pokazuje, że liczba wyjazdów jest nieco wyższa w dniach od piątku do poniedziałku.
-   **Liczba wyjazdów w zależności od miesiąca** ![image](https://github.com/user-attachments/assets/c20fa7d2-79a0-4252-87ce-f78dbd45963b)
  
 Wykres słupkowy obrazujący zmienność liczby wyjazdów w poszczególnych miesiącach roku, co pozwala na identyfikację trendów sezonowych w zapotrzebowaniu na pomoc medyczną. Na przedstawionym wykresie widać, że najwyższa liczba wyjazdów przypada na wakacje (lipiec, sierpień) oraz koniec roku (grudzień).
- **Heatmapa: Liczba wyjazdów według dnia tygodnia i godziny** ![image](https://github.com/user-attachments/assets/c454d9d0-ebfd-471e-8f0b-db106618fe41)


  Wykres łączący zależność od dni tygodnia oraz godziny. Możemy tu wyróżnić wzrost wezwań w godzinach przed południowych w poniedziałki oraz wieczorem w soboty.


**Wnioski:**
-   Piki wezwań w godzinach szczytu komunikacyjnego i wieczorem mogą sugerować zwiększoną liczbę wypadków lub nagłych zachorowań w tych okresach.
-   Różnice w obciążeniu w poszczególnych dniach tygodnia (np. wzrost w weekendy lub spadek w dni świąteczne) wskazują na potrzebę elastycznego planowania dyżurów.
-   Sezonowe wahania (np. wzrost w zimie związany z infekcjami) mogą wpływać na zapotrzebowanie na personel i sprzęt.

### 2. Analiza Przestrzenna 

Ta część analizy koncentruje się na geograficznym rozkładzie wezwań i miejsc, do których karetki dojeżdżają.

**Generowane wizualizacje (interaktywne mapy `folium` z wtyczkami oraz wykresy gęstości z `contextily`):**
-   **Mapa Ciepła (HeatMap) dla miejsc wezwań według powiatów** ![image](https://github.com/user-attachments/assets/62d04b7a-6caa-44f5-b03f-da1fb4c1a5b4)
Interaktywna mapa Folium z nałożoną warstwą HeatMap, która wizualizuje gęstość miejsc, w które karetki są wzywane. Ciemniejsze obszary wskazują na miejsca o największej koncentracji zdarzeń. Na załączonej mapie wyraźnie widać skupiska wezwań w Krakowie.
-   **Mapa Ciepła (HeatMap) dla miejsc wezwań według dzielnic miasta Krakowa** ![image](https://github.com/user-attachments/assets/bfb3f29d-076a-4b16-a4e4-310203bd3cd0)
  
 Analogicznie, HeatMap dla miasta Krakowa. Możemy tu wyróżnić dzielnice o największej gęstości zaludnienia.

**Wnioski:**
-   Heatmapy jasno identyfikują dzielnice lub obszary o największym zapotrzebowaniu na interwencje karetek, co jest kluczowe dla optymalnego rozmieszczenia baz pogotowia.
-   Identyfikacja obszarów, które generują dużą liczbę wezwań, jest kluczowa dla strategicznego rozmieszczenia karetek i skrócenia czasu dojazdu.


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



