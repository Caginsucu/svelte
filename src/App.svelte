<script>
	import Inputmask from 'inputmask';
	import creditCardType from 'credit-card-type';

	let backface = false,
	numberInput,
	expireInput,
	cvvInput;

	let card = {
		number: "",
		name: "",
		expire: "",
		cvv: "",
	}

	$: type = card.number ? creditCardType(card.number)?.[0]?.type : false

	$: if(numberInput && expireInput && cvvInput) {
		Inputmask({
			mask: "9999 9999 9999 9999",
			placeholder: " ",
		}).mask(numberInput)

		Inputmask({
			mask: "99/99",
			placeholder: " ",
		}).mask(expireInput)

		Inputmask({
			mask: "999",
			placeholder: " ",
		}).mask(cvvInput)
	}

	let addCard = false;
</script>

<br>
<br>

<div style="display:flex; justify-content: center">
	<div class="card" class:flip={backface}>
		<div class="front">
			<div class="card-top">
				<img src="images/chip.svg" alt="">
				{#if type}
				<img src="images/{type}.svg" alt="">
				{/if}
			</div>
			<div class="card-number">
				{card.number || '**** **** **** ****'}	
			</div>
			<div class="card-bottom">
				<div>
					<div class="key">Card Holder Name</div>
					<div class="value">{card.name || "***"}</div>
				</div>
				<div >
					<div class="key">Expire Date</div>
					<div class="value">{card.expire || "***"}</div>
				</div>
			</div>
		</div>
		<div class="back">
			<div class="card-back">
				CVV <em>{card.cvv || "***"}</em>
			</div>
		</div>
	</div>
</div>
<br>

<div style="display:flex;
flex-direction:column;">
	<input type="text" bind:this={numberInput} bind:value="{card.number}" placeholder="Card Numarası" > <br>
	<input type="text" bind:value="{card.name}" placeholder="Card Sahibi" > <br>
	<input type="text" bind:this={expireInput} bind:value="{card.expire}" placeholder="Son Kullanma Tarihi" > <br>
	<input type="text" bind:this={cvvInput} on:focus={() => backface = true} on:blur={() => backface = false} bind:value="{card.cvv}" placeholder="CVV" > <br>
</div>

<label>
	<input type="checkbox" bind:checked={backface}>
	Arka Yüzünü {backface ? "gizle": "göster"}
</label>


<button on:click={() => addCard = true}>Kaydet</button>
<br>
{#if addCard == true}
	{JSON.stringify(card)}
{/if}


<style>
	.card {
		width: 352px;
		height: 223px;
		position: relative;
		perspective: 800px;
	}

	.card .front, .card .back {
		width: inherit;
		height: inherit;
		background: linear-gradient(31.58deg, #93278F -2.49%, #29ABE2 67.92%);
		border-radius: 15px;
		padding: 31px 27px;


		display: flex;
		flex-direction: column;

		position: absolute;
		top: 0;
		left: 0;

		backface-visibility: hidden;
		transition: 1s all;
	}

	.card .front .card-top {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.card .front .card-number {
		font-size: 25px;
		color: #fff;
		margin-top: 28px;
		font-family: monospace;
	}

	.card .front .card-bottom {
		margin-top: auto;
		display: flex;
		justify-content: space-between;
		color: #fff;
	}

	.card .front .card-bottom .key {
		font-size: 12px;
		font-weight: 500;
		letter-spacing: -.3px;
		margin-bottom: 5px;
		opacity: .7;
	}

	.card .front .card-bottom .value {
		font-size: 15px;
		font-weight: 600;

	}


	.card .back .card-back {
		background: white;
		padding: 20px;
		margin-top: auto;
		display: flex;
		justify-content: end;
	}
	.card .back .card-back em {
		font-weight: bold;
		margin-left: 15px;
	}

	.card .back {
		transform: rotateY(180deg);
	}

	.card.flip .back {
		transform: rotateY(0);
	}
	.card.flip .front {
		transform: rotateY(-180deg);
	}
</style>