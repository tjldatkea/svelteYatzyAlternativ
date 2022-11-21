<script>
	let denAktiveSpillersIndex = 0;
	let showTotal = true;
	let resultArray = []; // [nEnere,nToere,nTreere,nFireere,nFemere,nSeksere]

	// Resultat er resultatet af terningekastet og muligvis det samme indhold som resultArray
	let resultat = {
		Enere: 0,
		Toere: 0,
		Treere: 0,
		Fireere: 0,
		Femere: 0,
		Seksere: 0,
	};
	let punktNavneListeUdenBonusOgTotal = [
		"Enere",
		"Toere",
		"Treere",
		"Fireere",
		"Femere",
		"Seksere",
		"Par",
		"ToPar",
		"TrePar",
		"TreEns",
		"FireEns",
		"ToGangeTreEns",
		"Lille",
		"Stor",
		"Royal",
		"Hus",
		"Chancen",
		"Yatzy",
	]; // "I plus eller minus" før Bonus kommer på

	let punktNavneListeMedMellemrum = [
		"Enere",
		"Toere",
		"Treere",
		"Fireere",
		"Femere",
		"Seksere",
		"Par",
		"To Par",
		"Tre Par",
		"Tre Ens",
		"Fire Ens",
		"To Gange Tre Ens",
		"Lille",
		"Stor",
		"Royal",
		"Hus",
		"Chancen",
		"Yatzy",
	]; // "I plus eller minus" før Bonus kommer på

	class Player {
		constructor(name, index) {
			this.name = name;
			this.playerIndex = index;
			this.playerHasBonus = false;
			this.playerCantGetBonus = false;
			this.resterendeSlag = 3;
			this.spillerenHarIndtastetPoint = false;
			this.punktTalListe = [];
			this.punktTalDict = {
				Enere: -1,
				Toere: -1,
				Treere: -1,
				Fireere: -1,
				Femere: -1,
				Seksere: -1,
				//Bonus: -1,
				Par: -1,
				ToPar: -1,
				TrePar: -1,
				TreEns: -1,
				FireEns: -1,
				ToGangeTreEns: -1,
				Lille: -1,
				Stor: -1,
				Royal: -1,
				Hus: -1,
				Chancen: -1,
				Yatzy: -1,
			};
		} // slut på constructor

		calculateTotal() {
			let sum = 0;
			for (const [key, value] of Object.entries(this.punktTalDict)) {
				if (value > -1) {
					sum += value;
				}
			}
			if (this.playerHasBonus) {
				sum += 50;
			}

			return sum;
		}

		makePunktTalDictAsAList() {
			this.punktTalListe = [];
			for (let i = 0; i < punktNavneListeUdenBonusOgTotal.length; i++) {
				this.punktTalListe.push(
					this.punktTalDict[punktNavneListeUdenBonusOgTotal[i]]
				);
			}
		}

		methodTest() {
			return "Test";
		}

		playersBeregnBonusEllerPlusEllerMinusOgReturnerStreng() {
			if (this.playerCantGetBonus) {
				return "0";
			}
			if (this.playerHasBonus) {
				return "50";
			}

			let bonusEllerPlusEllerMinus =
				this.playersBeregnBonusEllerPlusEllerMinus();
			if (bonusEllerPlusEllerMinus >= 0) {
				return "+" + bonusEllerPlusEllerMinus;
			} else {
				return "" + bonusEllerPlusEllerMinus;
			}
		}

		playersBeregnBonusEllerPlusEllerMinus() {
			let EnereTilSeksereListe = [
				this.punktTalDict.Enere,
				this.punktTalDict.Toere,
				this.punktTalDict.Treere,
				this.punktTalDict.Fireere,
				this.punktTalDict.Femere,
				this.punktTalDict.Seksere,
			];

			let EnereTilSeksereSum = 0;
			for (let i = 0; i < EnereTilSeksereListe.length; i++) {
				EnereTilSeksereSum +=
					EnereTilSeksereListe[i] >= 0 ? EnereTilSeksereListe[i] : 0;
			}

			if (EnereTilSeksereSum >= 84) {
				// Husk denne kan jo egentlig først bruges efter denne metode er blevet kaldt og EnereTilSeksereSum er højere end 84
				this.playerHasBonus = true;
				return 50;
			} else {
				if (
					this.punktTalDict.Enere > -1 &&
					this.punktTalDict.Toere > -1 &&
					this.punktTalDict.Treere > -1 &&
					this.punktTalDict.Fireere > -1 &&
					this.punktTalDict.Femere > -1 &&
					this.punktTalDict.Seksere > -1
				) {
					this.playerCantGetBonus = true;
					return 0;
				} else {
					return this.playersBeregnPlusMinus();
				}
			}
		}

		playersBeregnPlusMinus() {
			let EnereTilSeksereListe = [
				this.punktTalDict.Enere,
				this.punktTalDict.Toere,
				this.punktTalDict.Treere,
				this.punktTalDict.Fireere,
				this.punktTalDict.Femere,
				this.punktTalDict.Seksere,
			];

			console.log("EnereTilSeksereListe: " + EnereTilSeksereListe);

			let iPlusEllerMinus = 0;

			// For hvert punkt fra og med Enere til Seksere skal der tjekkes at der står noget andet end -1 og hvis der gør dette skal der tjekkes om der er mere eller mindre end 4xEnere-Seksere
			for (let i = 0; i < EnereTilSeksereListe.length; i++) {
				if (EnereTilSeksereListe[i] > -1) {
					// 4,8,12,16,20,24
					iPlusEllerMinus += EnereTilSeksereListe[i] - (i + 1) * 4;
					console.log("iPlusEllerMinus: " + iPlusEllerMinus);
				}
			}
			return iPlusEllerMinus;
		}

		skrivPointPaaPunkt(punktNummer, resultatPaaPunkt) {
			console.log("punktNummer: " + punktNummer);
			console.log("resultatPaaPunkt" + resultatPaaPunkt);
			this.punktTalDict[punktNavneListeUdenBonusOgTotal[punktNummer]] =
				resultatPaaPunkt;
			playerList = playerList;
		}
	} // slut på player klassen

	let numberOfPlayers = 2;
	let playerList = [];
	let tempNameList = ["Player One", "Player Two"];

	for (let i = 0; i < numberOfPlayers; i++) {
		playerList.push(new Player(tempNameList[i], i));
		playerList[i].makePunktTalDictAsAList();
	}

	class Dice {
		constructor(diceNumber) {
			this.diceNumber = diceNumber;
			this.dieId = "die" + diceNumber;
			this.dieClassText = "dice";
			this.holdDie = false;
			this.dieFaceValue = 0;
			this.diceVisible = true;
		}

		toggleHoldThisDie() {
			this.holdDie = !this.holdDie;
			this.dieClassText = this.holdDie ? "diceGreen" : "dice";
			diceArray = diceArray;
		}

		rollThisDie() {
			if (!this.holdDie) {
				this.dieFaceValue = Math.floor(Math.random() * 6) + 1;
				diceArray = diceArray;
			}
		}
	}

	let numberOfDice = 6;
	let diceArray = [];

	for (let i = 0; i < numberOfDice; i++) {
		diceArray.push(new Dice(i));
	}

	let beregnFunktionsListen = [
		beregnEnere,
		beregnToere,
		beregnTreere,
		beregnFireere,
		beregnFemere,
		beregnSeksere,
		beregnPar,
		beregnToPar,
		beregnTrePar,
		beregnTreEns,
		beregnFireEns,
		beregnToGangeTreEns,
		beregnLille,
		beregnStor,
		beregnRoyal,
		beregnHus,
		beregnChancen,
		beregnYatzy,
	];
	$: beregnFunktionsListen;

	function rollAllDice() {
		resultCalculationReady = false;

		let maxTimeout = 0;
		let timeout = 0;

		for (let i = 0; i < numberOfDice; i++) {
			if (!diceArray[i].holdDie) {
				timeout += 100;
			}
			if (!diceArray[i].holdDie) {
				setTimeout(() => {
					diceArray[i].diceVisible = false;
				}, timeout);
			}
		}

		timeout = 0;

		for (let i = 0; i < numberOfDice; i++) {
			if (!diceArray[i].holdDie) {
				timeout += 500;
				if (timeout > maxTimeout) {
					maxTimeout = timeout;
				}
			}
			if (!diceArray[i].holdDie) {
				setTimeout(() => {
					diceArray[i].diceVisible = true;
					isAllDiceIsVisible = checkIfAllDiceIsVisible(); // så kører den 6 gange og det er egentlig kun nødvendigt med en kørsel til sidst, så resultatet ikke kan aflæses på knapperne inden terningerne er synlige

					diceArray[i].rollThisDie();
				}, timeout);
			}
		}

		playerList[denAktiveSpillersIndex].resterendeSlag -= 1;

		setTimeout(() => {
			countOnesToSixes();

			playerList = playerList;
			diceArray = diceArray;
			beregnFunktionsListen = beregnFunktionsListen;

			resultCalculationReady = true;
		}, maxTimeout + 500);
	}

	let isAllDiceIsVisible = checkIfAllDiceIsVisible();

	let resultCalculationReady = false;

	$: isAllDiceIsVisibleAndResultCalculationReady =
		isAllDiceIsVisible && resultCalculationReady;

	function checkIfAllDiceIsVisible() {
		let allDiceVisible = true;

		for (let i = 0; i < numberOfDice; i++) {
			allDiceVisible = allDiceVisible && diceArray[i].diceVisible;
		}
		return allDiceVisible;
	}

	function toggleShowTotal() {
		showTotal = !showTotal;

		for (let i = 0; i < numberOfPlayers; i++) {
			playerList[i].calculateTotal();
		}

		playerList = playerList;
	}

	function changePlayer() {
		denAktiveSpillersIndex = (denAktiveSpillersIndex + 1) % numberOfPlayers;
		playerList[denAktiveSpillersIndex].resterendeSlag = 3;
		playerList[denAktiveSpillersIndex].spillerenHarIndtastetPoint = false;

		for (let i = 0; i < numberOfDice; i++) {
			diceArray[i].dieFaceValue = 0;
			if (diceArray[i].holdDie) {
				diceArray[i].toggleHoldThisDie();
			}
		}
		countOnesToSixes();
		beregnFunktionsListen = beregnFunktionsListen;
		playerList = playerList;
		diceArray = diceArray;
	}

	function resetResultat() {
		resultat.Enere = 0;
		resultat.Toere = 0;
		resultat.Treere = 0;
		resultat.Fireere = 0;
		resultat.Femere = 0;
		resultat.Seksere = 0;
		resultArray = [0, 0, 0, 0, 0, 0];
	}

	function countOnesToSixes() {
		resetResultat();

		for (let i = 0; i < diceArray.length; i++) {
			if (diceArray[i].dieFaceValue == 1) {
				resultat.Enere += 1;
			} else {
				if (diceArray[i].dieFaceValue == 2) {
					resultat.Toere += 1;
				} else {
					if (diceArray[i].dieFaceValue == 3) {
						resultat.Treere += 1;
					} else {
						if (diceArray[i].dieFaceValue == 4) {
							resultat.Fireere += 1;
						} else {
							if (diceArray[i].dieFaceValue == 5) {
								resultat.Femere += 1;
							} else {
								if (diceArray[i].dieFaceValue == 6) {
									resultat.Seksere += 1;
								}
							}
						}
					}
				}
			}
		} // slut på for løkken

		resultArray = [
			resultat.Enere,
			resultat.Toere,
			resultat.Treere,
			resultat.Fireere,
			resultat.Femere,
			resultat.Seksere,
		];
		console.log(resultArray);
	} // slut på funktionen

	function beregnEnere() {
		return resultat.Enere * 1;
	}

	function beregnToere() {
		return resultat.Toere * 2;
	}

	function beregnTreere() {
		return resultat.Treere * 3;
	}

	function beregnFireere() {
		return resultat.Fireere * 4;
	}

	function beregnFemere() {
		return resultat.Femere * 5;
	}

	function beregnSeksere() {
		return resultat.Seksere * 6;
	}

	function beregnPar() {
		if (
			resultat.Enere >= 2 ||
			resultat.Toere >= 2 ||
			resultat.Treere >= 2 ||
			resultat.Fireere >= 2 ||
			resultat.Femere >= 2 ||
			resultat.Seksere >= 2
		) {
			// Så er der i hvert fald et par og herfra skal så vælges det største
			if (resultat.Seksere >= 2) {
				return 12;
			} else {
				if (resultat.Femere >= 2) {
					return 10;
				} else {
					if (resultat.Fireere >= 2) {
						return 8;
					} else {
						if (resultat.Treere >= 2) {
							return 6;
						} else {
							if (resultat.Toere >= 2) {
								return 4;
							} else {
								if (resultat.Enere >= 2) {
									return 2;
								} else {
									return 0;
								}
							}
						}
					}
				}
			}
		} else {
			return 0;
		}
	}
	// med de indre indlejrede if'er, så kunne den ydre if betingelse nok droppes, men så skal man tjekke for par med Enere inden der returneres 2

	function beregnToPar() {
		return beregnNPar(2);
	}

	function beregnTrePar() {
		return beregnNPar(3);
	}

	function beregnNPar(n) {
		var nPairs = 0;
		var PairValueArray = [0, 0, 0];
		console.log("resultArray i  beregnNPar funktionen: " + resultArray);
		for (let i = 0; i < 6; i++) {
			if (resultArray[i] >= 2) {
				nPairs += 1;
				PairValueArray[nPairs - 1] = (i + 1) * 2; // faceValue = i+1
			}
		}

		if (n == 1 && nPairs >= 1) {
			// Værdien af et evt største par efterspørges og med nPairs >= 1 tjekkes der at der er et par
			return Math.max(
				PairValueArray[2],
				Math.max(PairValueArray[1], PairValueArray[0])
			); // Giver værdien af det største par
		} else {
			if (n == 2 && nPairs >= 2) {
				return Math.max(
					PairValueArray[1] + PairValueArray[0],
					PairValueArray[1] + PairValueArray[2]
				); // Giver værdien af de to største par
			} else {
				if (n == 3 && nPairs >= 3) {
					return PairValueArray[2] + PairValueArray[1] + PairValueArray[0]; // Giver værdien af 3 par
				} else {
					// Den sidste her er til at finde det mindste af 2 par, som bruges til at beregne værdien af et hus. Knapt så elegant at det er her, med n == 4.
					if (n == 4 && nPairs == 2) {
						return Math.min(PairValueArray[1], PairValueArray[0]); // Jeg ser bort fra PairValueArray[2], da der maks kan være to par, hvis der er et hus, men kan det gøres mere elegant? Bemærk at da jeg bruger Math.min, så vil den vælge nullet i PairValueArray[2] fremfor den mindste værdi af de to par der er gemt i de to forrige indexer/pladser i arrayet
					} else {
						return 0;
					}
				}
			}
		}
	}

	function beregnTreEns() {
		return beregnNGangeTreEns(1);
	}

	function beregnToGangeTreEns() {
		return beregnNGangeTreEns(2);
	}

	function beregnNGangeTreEns(n) {
		var nTriples = 0;
		var TripleValueArray = [0, 0];

		for (let i = 0; i < resultArray.length; i++) {
			if (resultArray[i] >= 3) {
				nTriples += 1;
				TripleValueArray[nTriples - 1] = (i + 1) * 3; // faceValue = i+1
			}
		}

		if (n == 1 && nTriples >= 1) {
			// Værdien af den evt største triple efterspørges og med nTriples >= 1 tjekkes der at der er et trippel
			return Math.max(TripleValueArray[1], TripleValueArray[0]); // Giver værdien af den største trippel
		} else {
			if (n == 2 && nTriples >= 2) {
				// større end ikke nødvendigt med 6 terninger
				return TripleValueArray[1] + TripleValueArray[0]; // Giver værdien af de to største triples
			} else {
				return 0;
			}
		}
	}

	function beregnFireEns() {
		if (
			resultat.Enere >= 4 ||
			resultat.Toere >= 4 ||
			resultat.Treere >= 4 ||
			resultat.Fireere >= 4 ||
			resultat.Femere >= 4 ||
			resultat.Seksere >= 4
		) {
			// Så er der 4 ens
			if (resultat.Seksere >= 4) {
				return 4 * 6;
			} else {
				if (resultat.Femere >= 4) {
					return 4 * 5;
				} else {
					if (resultat.Fireere >= 4) {
						return 4 * 4;
					} else {
						if (resultat.Treere >= 4) {
							return 4 * 3;
						} else {
							if (resultat.Toere >= 4) {
								return 4 * 2;
							} else {
								if (resultat.Enere >= 4) {
									return 4 * 1;
								}
							}
						}
					}
				}
			}
		} else {
			return 0;
		}
	}

	function beregnLille() {
		if (
			resultat.Enere >= 1 &&
			resultat.Toere >= 1 &&
			resultat.Treere >= 1 &&
			resultat.Fireere >= 1 &&
			resultat.Femere >= 1
		) {
			return 15;
		} else {
			return 0;
		}
	}

	function beregnStor() {
		if (
			resultat.Toere >= 1 &&
			resultat.Treere >= 1 &&
			resultat.Fireere >= 1 &&
			resultat.Femere >= 1 &&
			resultat.Seksere >= 1
		) {
			return 20;
		} else {
			return 0;
		}
	}

	function beregnRoyal() {
		if (
			resultat.Enere == 1 &&
			resultat.Toere == 1 &&
			resultat.Treere == 1 &&
			resultat.Fireere == 1 &&
			resultat.Femere == 1 &&
			resultat.Seksere == 1
		) {
			return 30;
		} else {
			return 0;
		}
	}

	function beregnHus() {
		// Brug beregnNPar(2) og beregnTreEns() hvor et positivt resultat fra begge indikerer at der er et hus, herefter skal værdien bestemmes
		// Tag herefter det største trippel(der kan jo være 2x3ens) og det mindste af de 2 par, så skulle man gerne have værdien af huset

		if (beregnNPar(2) > 0 && beregnTreEns() > 0) {
			// Så er der et hus
			return beregnTreEns() / 3 + beregnNPar(2);
		} else {
			return 0;
		}
	}

	function beregnChancen() {
		let sumOfDice = 0;
		for (let i = 0; i < numberOfDice; i++) {
			sumOfDice += diceArray[i].dieFaceValue;
		}
		return sumOfDice;
	}

	function beregnYatzy() {
		let d1 = diceArray[0].dieFaceValue;
		let d2 = diceArray[1].dieFaceValue;
		let d3 = diceArray[2].dieFaceValue;
		let d4 = diceArray[3].dieFaceValue;
		let d5 = diceArray[4].dieFaceValue;
		let d6 = diceArray[5].dieFaceValue;

		if (d1 == d2 && d2 == d3 && d3 == d4 && d4 == d5 && d5 == d6) {
			if (d1 == 1) {
				return 100;
			} else {
				if (d1 != 0) {
					return d1 * 6 + 50;
				} else {
					return 0;
				}
			}
		} else {
			return 0;
		}
	}

	import { fade } from "svelte/transition";
</script>

<main>
	<!-- <div id="spilDiven"> -->
	<div id="upperDiv">
		<div id="DiceDiv" class="prevent-select">
			<table>
				<tr>
					{#each diceArray as die, i}
						<td class="diceTd">
							{#if die.diceVisible}
								<div
									id={die.dieId}
									on:click={() => die.toggleHoldThisDie()}
									class={die.dieClassText}
									transition:fade
								>
									{die.dieFaceValue}
								</div>
							{/if}
						</td>
					{/each}
				</tr>
			</table>
		</div>
		<!-- slut på DiceDiv -->
		<p id="slag">
			Spillers tur: <br /><b> {playerList[denAktiveSpillersIndex].name}</b>
			<br />
			<br />
			Slag: <b>{playerList[denAktiveSpillersIndex].resterendeSlag}</b>
		</p>
		<button id="rollDiceButton" on:click={() => rollAllDice()}
			>Kast terninger</button
		>
		<button id="changePlayerButton" on:click={() => changePlayer()}
			>Giv bæger videre</button
		>
	</div>
	<!-- slut på upper div -->
	<!-- Slut på spilDiven, men bør den ikke være om det hele??? -->

	<table>
		<tr>
			<th />
			<th>{playerList[denAktiveSpillersIndex].name}</th>
			<th />
			<th />
			{#each playerList as player, k}
				<th>{playerList[k].name}</th>
			{/each}
			<th />
		</tr>
		{#each punktNavneListeUdenBonusOgTotal as punktNavn, j}
			<tr>
				<td>
					{punktNavneListeMedMellemrum[j]}
				</td>
				<td>
					{playerList[denAktiveSpillersIndex].punktTalDict[
						punktNavneListeUdenBonusOgTotal[j]
					] > -1
						? playerList[denAktiveSpillersIndex].punktTalDict[
								punktNavneListeUdenBonusOgTotal[j]
						  ]
						: ""}
				</td>

				<td>
					{#if playerList[denAktiveSpillersIndex].punktTalDict[punktNavneListeUdenBonusOgTotal[j]] == -1}
						<button
							class="slimButton"
							on:click={() =>
								playerList[denAktiveSpillersIndex].skrivPointPaaPunkt(
									j,
									beregnFunktionsListen[j]()
								)}
						>
							Skriv {isAllDiceIsVisibleAndResultCalculationReady
								? beregnFunktionsListen[j]()
								: "?"} På {punktNavneListeMedMellemrum[j]}
						</button>
					{/if}
				</td>
				<td>
					<button
						class="slimButton"
						on:click={() =>
							playerList[denAktiveSpillersIndex].skrivPointPaaPunkt(j, -1)}
					>
						Nulstil Punkt
					</button>
				</td>
				{#each playerList as player, i}
					<td>
						{player.punktTalDict[punktNavneListeUdenBonusOgTotal[j]] > -1
							? player.punktTalDict[punktNavneListeUdenBonusOgTotal[j]]
							: ""}
					</td>
				{/each}
				<td>
					{punktNavneListeMedMellemrum[j]}
				</td>
			</tr>
			{#if j == 5}
				<tr>
					<td>
						{playerList[denAktiveSpillersIndex].playerCantGetBonus ||
						playerList[denAktiveSpillersIndex].playerHasBonus
							? "Bonus"
							: "I plus eller minus"}
					</td>
					<td>
						{playerList[
							denAktiveSpillersIndex
						].playersBeregnBonusEllerPlusEllerMinusOgReturnerStreng()}
					</td>
					<td /><td />
					{#each playerList as player, m}
						<td>
							{player.playersBeregnBonusEllerPlusEllerMinusOgReturnerStreng()}
						</td>
					{/each}
					<td>
						{playerList[denAktiveSpillersIndex].playerCantGetBonus ||
						playerList[denAktiveSpillersIndex].playerHasBonus
							? "Bonus"
							: "I plus eller minus"}
					</td>
				</tr>{/if}

			{#if j == 17}
				<tr>
					<td>
						{"Total"}
					</td>
					<td>
						{showTotal
							? playerList[denAktiveSpillersIndex].calculateTotal()
							: ""}
					</td>
					<td>
						<button on:click={() => toggleShowTotal()}>
							{showTotal ? "Vis ikke total" : "Vis total"}
						</button>
					</td>
					<td />
					{#each playerList as player, k}
						<td>{showTotal ? playerList[k].calculateTotal() : ""}</td>
					{/each}
					<td>
						{"Total"}
					</td>
				</tr>
			{/if}
		{/each}
	</table>
</main>

<style>
	div.dice {
		float: left;
		width: 64px;
		background: #f5f5f5;
		border: #999 2px solid;
		padding: 20px;
		font-size: 48px;
		text-align: center;
		margin: 10px;
		border-radius: 15px;
	}

	div.diceGreen {
		float: left;
		width: 64px;
		background: #00ff00;
		border: #999 2px solid;
		padding: 20px;
		font-size: 48px;
		text-align: center;
		margin: 10px;
		border-radius: 15px;
	}

	main {
		font-family: "Lucida Console", "Courier New", monospace;
	}

	table {
		font-family: arial, sans-serif;
		border-collapse: collapse;
	}

	td,
	th {
		border: 1px solid #dddddd;
		text-align: center;
		height: 16px;
		width: 10%;
	}

	tr:nth-child(even) {
		background-color: #dddddd;
	}

	td.diceTd {
		height: 131px;
		width: 134px;
		border: none;
	}

	button.slimButton {
		text-align: center;
		height: 1.7em;
		width: 18em;
		font-family: "Lucida Console", "Courier New", monospace;
		font-size: 0.75em;
		border-radius: 10px;
	}

	#DiceDiv {
		display: flex;
		justify-content: space-around;
	}

	#upperDiv {
		display: flex;
		justify-content: space-around;
	}

	.prevent-select {
		-webkit-user-select: none; /* Safari */
		-ms-user-select: none; /* IE 10 and IE 11 */
		user-select: none; /* Standard syntax */
	}
</style>
