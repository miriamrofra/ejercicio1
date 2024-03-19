	private final static String[] UNIDADES = { "cero", "un", "dos", "tres", "cuatro", "cinco", "seis", "siete", "ocho",
			"nueve" };
	private final static String[] DECENAS = { "diez", "once", "doce", "trece", "catorce", "quince", "dieciseis",
			"diecisiete", "dieciocho", "diecinueve", "veinte", "treinta", "cuarenta", "cincuenta", "sesenta", "setenta",
			"ochenta", "noventa" };
	private final static String[] CENTENAS = { "ciento", "doscientos", "tresciento", "cuatrocientos", "quinientos",
			"seiscientos", "setecientos", "ochocientos", "novecientos" };

	public static String convert(double amount) {

		String solEntero = null;
		String solDecimal = null;
		String numeroAString = String.valueOf(amount);
		String s[] = numeroAString.split("\\.");

		String parteEntera = s[0];
		String parteDecimal = s[1];

		if (parteEntera.length() < 4) {

			// unidades
			if (parteEntera.length() == 1)
				solEntero = getUnidades(parteEntera);

			// decenas
			else if (parteEntera.length() == 2)
				solEntero = getDecenas(parteEntera);
			// centenas

			else
				solEntero = getCentenas(parteEntera);
		} else {

			solEntero = getMiles(parteEntera);

		}

		if (parteDecimal.equalsIgnoreCase("0")) {
			System.out.println(solEntero + " euros");
		} else {
			if (parteDecimal.length() == 1) {
				solDecimal = getUnidades(parteDecimal);
			} else if (parteDecimal.length() == 2) {
				solDecimal = getDecenas(parteDecimal);
			}

			System.out.println(solEntero + " euros con " + solDecimal + " centimos");
		}

		return "";
	}

	public static String getUnidades(String num) {
		return UNIDADES[Integer.parseInt(num)];
	}

	public static String getDecenas(String num) {

		int numInt = Integer.parseInt(num);
		int unidades = numInt % 10;
		int decenas = (numInt / 10) % 10;

		if (numInt < 10) {
			return getUnidades(numInt + "");
		} else if (numInt >= 10 && numInt < 20) {

			return DECENAS[numInt - 10];

		} else {

			if (unidades == 0) {
				return DECENAS[decenas + 8];
			} else {
				if (decenas == 2) {

					return "veinti" + UNIDADES[unidades];

				} else {
					return DECENAS[decenas + 8] + " y " + UNIDADES[unidades];
				}
			}

		}

	}

	public static String getCentenas(String num) {

		int numInt = Integer.parseInt(num);
		int unidades = numInt % 10;
		int decenas = (numInt / 10) % 10;
		int centenas = (numInt / 10) / 10;

		if (decenas == 0 && unidades == 0) {
			if (centenas == 1)
				return "cien";
			else
				return CENTENAS[centenas - 1];
		} else {

			String decenasS = getDecenas(num.substring(1, 3));
			return CENTENAS[centenas - 1] + " " + decenasS;

		}

	}

	public static String getMiles(String num) {

		int numInt = Integer.parseInt(num);
		int miles = numInt / 1000;
		int centenas = numInt % 1000;
		String res = null;

		if (miles < 100 && miles > 10)
			res = getDecenas(miles + "") + " mil ";
		else if (miles < 10)
			res = getUnidades(miles + "") + " mil ";
		else
			res = getCentenas(miles + "") + " mil ";

		if (centenas != 0) {
			if (centenas < 100 && centenas > 10)
				res += getDecenas(centenas + "");
			else if (centenas < 10)
				res += getUnidades(centenas + "");
			else
				res += getCentenas(centenas + "");
		}

		return res;

	}

	public static void main(String[] args) {

		convert(91991.99);
	}
