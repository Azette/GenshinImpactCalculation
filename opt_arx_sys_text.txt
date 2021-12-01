const n = 30; //Number of rolls

const Ab = 726; //Base attack

const Af = 422; //Flat attack

const abo = 1.059; //%ATK bonus

const rbo = 0.365; //%Crit Rate bonus

const xbo = 0.5; //%Crit DMG bonus

const dbo = 1.07; //%DMG bonus

const fa = 0.5; //%ATK roll factor

const fr = 0.5; //%Crit Rate roll factor

const fx = 0.5; //%Crit DMG roll factor



const inArr = [n, Ab, Af, abo, rbo, xbo, fa, fr, fx, dbo];



function opt_arx(arg) {

    const psia = 0.041 + 0.017*arg[6];

    const psir = 0.027 + 0.012*arg[7];

    const psix = 0.054 + 0.024*arg[8];

    const V = arg[0] + (1+arg[3]+arg[2]/arg[1])/psia + arg[4]/psir + arg[5]/psix;

    const Vmin = Math.sqrt(12/(psix*psir));
    
	if (V < Vmin) {
        
		return "Your base stats is too low";
    
	}

    let nr = 1/6*(V + Math.sqrt(Math.pow(V, 2) - 12/(psir*psix))) - arg[4]/psir;

    let r = nr*psir + arg[4];

    let na = 0;

    let nx = 0;

    if (r > 1) {

        r = 1;

        nr = (1 - arg[4])/psir;

        na = 1/2*(arg[0] - nr + (arg[5]-1)/psix - (arg[3]+arg[2]/arg[1]-1)/psia);

        nx = arg[0] - nr - na;
    } else {
        na = 1/3*(2*V - Math.sqrt(Math.pow(V, 2) - 12/(psir*psix))) - (1+arg[3]+arg[2]/arg[1])/psia;

        nx = 1/6*(V + Math.sqrt(Math.pow(V, 2) - 12/(psir*psix))) - arg[5]/psix;

    }

    const a = na*psia + arg[3];

    const x = nx*psix + arg[5];

    const D = (arg[1]*(1 + a) + arg[2])*(1 + r*x)*(1+arg[arg.length - 1]);

    outArr = [a, r, x, D];

    return outArr;

}



console.log(opt_arx(inArr));