## Enkel Snitt Kalkulator

Dette er en enkel snitt kalkulator som lar deg beregne ditt gjennomsnitt basert på karakterer og studiepoeng. Du kan bruke kalkulatoren ved å følge disse trinnene:

1. Logg inn på [Samordnaopptak](https://sok.samordnaopptak.no/).
2. Naviger fra "Gå til dokumentasjon" til Høyre utdanning, eller trykk [her](https://sok.samordnaopptak.no/#/documentation).
3. Kopier hele tabellen som er vist under og lim den inn i tekstfeltet.
4. Trykk på "Beregn snittet mitt".

### Kodeforklaring

Denne kalkulatoren er implementert ved hjelp av JavaScript. Her er en kort forklaring av koden:

```javascript
       // Grade values mapping
        const gradeValues = {
            A: 5,
            B: 4,
            C: 3,
            D: 2,
            E: 1,
            F: 0
        };

        // Array to store grade objects
        let gradeArray = [];

        // Calculation function triggered by input change or button click
        const calculate = () => {
            // Retrieve input text from textarea
            const text = document.getElementById("inputField").value;

            // Check if the input field is empty
            if (text.trim() === "") {
                alert("Vennligst fyll ut karakterfeltet.");
                return;
            }

            // Variables to store final grade and number of elements
            let tempGrade = 0;
            let tempWeigth = 0.0;
            let totalWeigth = 0.0;


            // Split the text into rows
            const rows = text.split('\n');

            // Determine the starting index for iteration based on the presence of a header row
            let i = 0;
            if (text.includes("Emnekode")) {
                i = 1;
            }

            // Iterate over each row starting from index 1 (to skip the header row) or 0 if the header row is not present
            for (; i < rows.length; i++) {
                const columns = rows[i].split('\t');
                const aWeight = columns[3];
                const aGrade = columns[4];

                // Push the values to the array as an object
                const obj = {
                    weight: parseFloat(aWeight),
                    grade: aGrade
                };
                gradeArray.push(obj);
            }

            // Calculate the final grade by multiplying grade value and weight for each object in the array
            for (let obj of gradeArray) {
                const grade = gradeValues[obj.grade] * obj.weight;
                totalWeigth += obj.weight;
                // Source https://www.oslomet.no/studier/soknad-og-opptak/poengberegning-rangeringsregler

                // Check if the grade is a valid number
                if (!isNaN(grade) && Number.isInteger(grade)) {
                    tempGrade += grade;
                    tempWeigth += obj.weight;
                }
            }
            // Calculate the average grade and display the result
            const finalGrade = ((tempGrade / tempWeigth)).toFixed(2);
            document.getElementById("result").innerHTML = `Voilà!😎 Ditt snitt er ${finalGrade} av totalt ${totalWeigth} studie poeng`;

            // Reset the gradeArray for future calculations
            gradeArray = [];
        }

        // Event listeners for input change and button click
        document.addEventListener("DOMContentLoaded", () => {
            const inputField = document.getElementById("inputField");
            inputField.addEventListener("input", calculate);
            const calculateButton = document.getElementById("calculateButton");
            calculateButton.addEventListener("click", calculate);
        });
```

### Brukerveiledning

1. Først må du logge inn på [Samordnaopptak](https://sok.samordnaopptak.no/).
2. Deretter navigerer du fra "Gå til dokumentasjon" til Høyre utdanning, eller du kan klikke [her](https://sok.samordnaopptak.no/#/documentation).
3. Kopier tabellen som er vist i instruksjonene og lim den inn i tekstfeltet på kalkulatoren.
4. Klikk på knappen "Beregn snittet mitt".

### Lisens og opphavsrett

GNU General Public License v3.0

Gjerne send [meg](mailto:rasmus.skramstad@gmail.com?subject=Feil%20p%C3%A5%20Enkel%20Snitt%20kalkulator) en beskjed dersom du finner noe feil hvis du opplever noen feil eller har spørsmål.

Merk: Ingen av karakterene blir lagret eller sendt. Siden er ikke tilknyttet Samordna Opptak. Den er laget fordi jeg selv var for lat til å regne det ut selv. Karakterene er kalkulert ved hjelp av en formel for vektet gjennomsnitt, hvor karakterene multipliseres med antall studiepoeng og deretter deles på antall studiepoeng. Formelen er hentet fra [OsloMet 2023](https://www.oslomet.no/studier/soknad-og-opptak/poengberegning-rangeringsregler).
