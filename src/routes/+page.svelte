<script lang="ts">
	let romanNumeral: string = $state('');
	let decimalResult: number | undefined = $state();

	let decimal: number | undefined = $state();
	let romanResult: string = $state('');

	const romanToDecimal = () => {
		romanNumeral = romanNumeral.toUpperCase().replace(/[^IVXLCDM]/g, '');
		const romanNumerals: { [key: string]: number } = {
			I: 1,
			V: 5,
			X: 10,
			L: 50,
			C: 100,
			D: 500,
			M: 1000
		};

		let total = 0;
		let i = 0;

		while (i < romanNumeral.length) {
			const current = romanNumerals[romanNumeral[i]];
			const next = romanNumerals[romanNumeral[i + 1]];

			if (current < next) {
				total += next - current;
				i++;
			} else {
				total += current;
			}

			i += 1;
		}

		decimalResult = total === 0 ? undefined : total;
	};

	const decimalToRoman = () => {
		const romanNumerals: { [key: number]: string } = {
			1: 'I',
			5: 'V',
			10: 'X',
			50: 'L',
			100: 'C',
			500: 'D',
			1000: 'M'
		};

		let _decimal = decimal || 0;
		let total = '';

		while (_decimal > 0) {
			const current: string = Object.keys(romanNumerals)
				.reverse()
				.find((key) => Number(key) <= _decimal)!;
			total += romanNumerals[Number(current)];
			_decimal -= Number(current);
		}

		romanResult = total
			.replace(/VIIII/g, 'IX')
			.replace(/XXXXX/g, 'XC')
			.replace(/CCCCC/g, 'CM')
			.replace(/IIII/g, 'IV')
			.replace(/XXXX/g, 'XL')
			.replace(/CCCC/g, 'CD');
	};
</script>

<div>
	<h1>Roman Numerals Converter</h1>
	<img src="/ixii.gif" alt="avatar" />
	<p>Convert Roman Numerals to Decimal</p>
	<input
		type="text"
		placeholder="Enter Roman Numerals"
		bind:value={romanNumeral}
		oninput={romanToDecimal}
	/>
	<p>
		{romanNumeral} = {decimalResult}
	</p>

	<p>Convert Decimal to Roman Numerals</p>
	<input type="text" placeholder="Enter Decimal" bind:value={decimal} oninput={decimalToRoman} />
	<p>
		{decimal} = {romanResult}
	</p>
</div>

<style>
	img {
		max-width: min(40rem, 90%);
		max-height: min(40rem, 40%);
		border-radius: 3rem;
		width: 90%;
		height: 40%;
		object-fit: cover;
	}
</style>
