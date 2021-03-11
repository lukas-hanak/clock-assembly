org 0

;
;	INIT
;
zac: 	mov	r0,#0 ;sekundy
	mov	r1,#0 ;minuty
	mov	r2,#0 ;hodiny
	mov	r3,#0 ;min budiku
	mov	r4,#0 ;hodin budiku

	call	zobraz
	jmp	klavesnice

start:
	mov	a,p1	;klavesnice
	cpl	a	;negace	p1 v acc
	cjne	a,#2,start_1
	jmp	zobrat_min_sek

start_1:
	call	zobraz	;zobrazime hodiny
	; na konci podprogramu zobraz skaceme na start_2

start_bud_1:
	cjne	a,#5,start_bud_2
	call	zobraz

start_bud_2:
	cjne	a,#5,zobraz_budik
	jmp	zobraz_budik_pr

start_2:
	jmp	vypocet
	call	zp	;zpomalime
	;sekundy
	inc	r0	;sekundy + 1
	cjne	r0,#60,start
	mov	r0,#0
	;minuty
	inc	r1
	cjne	r1,#60,start
	mov	r1,#0
	;hodiny
	inc	r2
	cjne	r2,#24,start
	mov	r2,#0

	jmp	start

vypocet:
	;hodiny
	mov	a,r2	;hodiny
	mov	b,r4	;hodiny budiku

	clr	cy	;vynulovani cy
	subb	a,b	;a-b-cy
	jnz	start_2

	;minuty
	mov	a,r1	;min
	mov	b,r3	;minuty budiku
	clr	cy	;vynulovani cy
	subb	a,b	;a-b-c
	jnz	blikani

blikani:
	jnb	p1.3,start	; bit 3 = kl 8
	mov	p3,#00111111b
	mov	p3,#00101111b
	mov	p3,#00011111b
	mov	p3,#00001111b
	call	zp
	call	zobraz
	call	zp
	jmp	blikani
;
;	NASTAVENI
;
nastav_minut:
	inc	r1
	call	zobraz
	cjne	r1,#59,nas_1
	mov	r1,#255
nas_1:	jmp	klavesnice

nastav_hodin:
	inc	r2
	call	zobraz
	cjne	r2,#23,nas_2
	mov	r2,#255
nas_2:	jmp	klavesnice

nastav_min_budiku:	
	inc	r3
	call	zobraz_budik
	cjne	r3,#59,nas_3
	mov	r3,#255
nas_3:	jmp	klavesnice

nastav_hod_budiku:
	inc	r4
	call	zobraz_budik
	cjne	r4,#23,nas_4
	mov	r4,#255
nas_4:	jmp	klavesnice

;
;	ZOBRAZENI
;
zobraz_budik:
	; r4 - hodiny budiku
	mov	a,r4
	mov	a,b
	orl	a,#00110000b
	mov	p3,a

	mov	p3,a
	mov	a,b
	orl	a,#00100000b
	mov	p3,a

	; r3 - minuty budiku
	mov	a,r3
	mov	b,#10
	div	ab
	orl	a,#00010000b
	mov	p3,a
	mov	a,b
	orl	a,#00000000b
	mov	p3,a
	ret

zobraz_budik_pr:
	; r4 - hodiny budiku
	mov	a,r4
	mov	a,b
	orl	a,#00110000b
	mov	p3,a

	mov	p3,a
	mov	a,b
	orl	a,#00100000b
	mov	p3,a

	; r3 - minuty budiku
	mov	a,r1
	mov	b,#10
	div	ab
	orl	a,#00010000b
	mov	p3,a
	mov	a,b
	orl	a,#00000000b
	mov	p3,a

zobrat_min_sek:
	; r1 - minuty
	mov	a,r1
	mov	a,b
	orl	a,#00110000b
	mov	p3,a
	mov	p3,a
	mov	a,b
	orl	a,#00100000b
	mov	p3,a

	; r0 - sekundy
	mov	a,r0
	mov	b,#10
	div	ab
	orl	a,#00010000b
	mov	p3,a
	mov	a,b
	orl	a,#00000000b
	mov	p3,a
	ret

zobraz:
	; r2 - hodiny
	mov	a,r2
	mov	a,b
	orl	a,#00110000b
	mov	p3,a

	mov	p3,a
	mov	a,b
	orl	a,#00100000b
	mov	p3,a

	; r1 - minuty
	mov	a,r1
	mov	b,#10
	div	ab
	orl	a,#00010000b
	mov	p3,a
	mov	a,b
	orl	a,#00000000b
	mov	p3,a
	jmp	start_2

;
;	KLAVESNICE
;
klavesnice:
	;jnb	p1.0,nastav_minut
	;jnb	p1.2,nastav_hodin
	;jmp	klavesnice
	;nastav_min_budiku
	;nastav_hod_budiku
	;nulovani
	;start hod
	mov	a,p1 ;prectu klavesnice v portu p1
	cpl	a    ;negace acc
	cjne	a,#1,kl_2
	call	nastav_minut

kl_2:	cjne	a,#4,kl_3
	call	nastav_hodin

kl_3:	cjne	a,#3,kl_4
	call	nastav_min_budiku

kl_4:	cjne	a,#6,kl_5
	call	nastav_hod_budiku

kl_5:	cjne	a,#32,kl_6
	jmp	zac 	;nulovani

kl_6:	cjne	a,#16,kl_7
	jmp	start

kl_7:	jmp	klavesnice

;
;	ZPOMALENI
;
zp:	djnz	r7,$
	djnz	r6,$-2
	ret	

	end