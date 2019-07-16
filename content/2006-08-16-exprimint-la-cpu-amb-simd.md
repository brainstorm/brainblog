---
author: brainstorm
date: '2006-08-15T14:00:00.000000+00:00'
dsq_thread_id:
- 2874888866
excerpt: null
layout: post
modified: '2006-08-15T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/08/16/exprimint-la-cpu-amb-simd/
tags:
- bio
- software
- university
title: Exprimint la CPU amb SIMD
---

Fa un temps vaig [comentar][1] que parlaria més o menys regularment de BioDAC... bé, el cert és que durant el curs ja he anat prou atabalat i no m'hi he pogut dedicar tot el que voldria, però ara sí :) 

Un dels objectius bàsics que comentava en el post inicial de la beca era: 

> (...) optimitzar tècniques de pattern matching usant instruccions [SIMD][2] presents en els microprocessadors d’avui en dia

Doncs bé, això he fet aquests dies de sol i pluja d'estiu, tot alternant entre platja i tecles ;) 

### Què és SIMD ?

En poques paraules: procesar amb una sola instrucció, múltiples dades, d'aqui ***S**ingle **I**nstruction **M**ultiple **D**ata*. No explicaré més a fons els detalls d'aquesta tecnologia, trobo que wikipedia ja em substitueix prou bé :) Només volia comentar uns detalls que m'he trobat al treballar-hi i que està bé tenir en compte si decidiu traduïr el vostre flamant algorisme per a que usi registres vectorials...

<!--more-->

#### Referències

1.  Si, tothom sap que van al final de qualsevol post, però millor començar per quina [API][3] tenim, no ? He posat aquesta primer perque considero que és la més ràpidament/còmodament consultable.
2.  També podeu tirar dels [llibres blaus-blancs][4] de referència d'intel sobre instruccions d'asm, força més farragosos però extremadament detallats (ideals per mirar latències/temps d'instruccions).
3.  Al projecte usem GCC, podeu mirar directament els prototips als seus headers al CVS o al vostre sistema (en el meu cas a: /usr/lib/gcc-lib/i686-pc-linux-gnu/3.3.5/include):
*   [SSE][5]
*   [SSE2][6]
*   [SSE3][7]

#### En ensamblador **no**: usem les *intrinsics*

Tots sabem que resulta poc elegant/portable/mantenible incrustar asm dins d'un codi en C (amb honroses excepcions). Per això tenim les intrinsics de gcc, funcions que encapsulen les instruccions vectorials en ensamblador, un petit exemple d'intrinsics seria el següent:

<pre>%:include &lt;stdlib.h&gt;
%:include &lt;xmmintrin.h&gt;

void
vector_add(int *A, int *B, int *C, int len, int iter)
{
    int i,j;
    __m128i *a, *b, *c;

   for (j=0; j &lt; iter ; j++)
    for (i=0; i &lt; len; i+=4) {
        a = (__m128i*) &A[i];
        b = (__m128i*) &B[i];
        c = (__m128i*) &C[i];

        *c = _mm_add_epi32(*a, *b);
    }
}

int main()
{
    int A[32] __attribute__((aligned(16)));
    int B[32] __attribute__((aligned(16)));
    int C[32] __attribute__((aligned(16)));
    unsigned int i;

    for (i=0; i&lt;32; i++) A[i]=B[i]=i;

    vector_add(A,B,C,32,1);
    return 0;
}
</pre>

És un codi d'exemple de l'assignatura [<acronym title="Programació Conscient de l'Arquitectura">PCA</acronym>][8] de la FIB, per si us interessa el tema. El que fa és sumar els valors dels vectors A & B en el C, però ho fa de 128 bits en 128 bits mitjançant [**\_mm\_add_epi32**][9], en comptes de 32 a 32 com estariem acostumats.

#### x<<1 en vectorial ? Uff

Les instruccions vectorials estan pensades per a multimèdia, per a processar moltes dades de cop. És possible que per aquesta raó no van incloure el shift d'un bit d'un registre vectorial sencer, com a mínim s'ha de moure un byte (8 bits) de cop :-/ Tot i això hi ha *workarounds* per fer moviments de bits individuals. Al que hem arribat a la beca de formació és aquest (exemple de shift left d'1 bit):

<pre>inline __m128i mm_slli_bit_1 (const __m128i vect) {

    __m128i b,c;

    c = _mm_and_si128(vect,shift_bit_masks[1]);

    b = _mm_slli_si128(c,1);
    b = _mm_srli_epi64(b,7);
    c = _mm_slli_epi64(vect,1);

    return _mm_or_si128(c,b);
}
</pre>

S'accepten millores/optimitzacions d'aquest últim ;)

 [1]: https://blogs.nopcode.org/brainstorm/2005/10/02/beca-de-formacio-biodac/
 [2]: https://en.wikipedia.org/wiki/SIMD
 [3]: https://www.nersc.gov/vendor_docs/intel/c_ug/whgdata/whlstt43.htm
 [4]: https://www.intel.com/design/pentium4/manuals/248966.htm
 [5]: https://gcc.gnu.org/viewcvs/trunk/gcc/config/i386/xmmintrin.h?view=markup
 [6]: https://gcc.gnu.org/viewcvs/trunk/gcc/config/i386/emmintrin.h?view=markup
 [7]: https://gcc.gnu.org/viewcvs/trunk/gcc/config/i386/pmmintrin.h?view=markup
 [8]: https://www.fib.upc.edu/ca/Estudis/Assignatures/PCA.html
 [9]: https://www.nersc.gov/vendor_docs/intel/c_ug/comm1046.htm