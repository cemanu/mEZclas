#include <stdio.h>
#include <math.h>
#include <limits.h>
#define PI 3.1416

void leer_rango(int*, int*);
int validar_si_esta_en_rango(int, int, int);
double calcular_resistencia_en_MPa(double, double, double, double, char);
void hallar_max__min(double*r_max, double*r_min, char mezcla, char unidad_de_medida, double d_sup, double d_med, double d_inf, double F, int r_inicial, int r_final, char *mezcla_min, char *mezcla_max);

int main(){
	int r_inicial, r_final;
	double res_en_MPa, F, d_sup, d_inf, d_med, r_min, r_max;
	char unidad_de_medida, identificador, mezcla_menor, mezcla_mayor;
	leer_rango(&r_inicial, &r_final);
	if(r_inicial>=r_final){
		printf("Debe ingresar un rango valido (valor inicial < valor final)");
		return 0;	
	}
	else{
		r_max=0; 
		r_min=INT_MAX;
		while(1){
			printf("Ingrese el identificador: ");
			scanf("%c", &identificador);
			printf("Indique la unidad de medida: ");
			scanf("%c", &unidad_de_medida);
			printf("Ingrese el diametro superior, inferior y medio, y la  carga: ");
			scanf("%lf %lf %lf %lf",&d_sup, &d_inf, &d_med, &F);
			if(d_sup>0&&d_med>0&&d_inf>0&&F>0){
				hallar_max__min(&r_max, &r_min, identificador, unidad_de_medida, d_sup, d_med, d_inf, F, r_inicial, r_final, &mezcla_menor, &mezcla_mayor);
			}
			if(d_sup==0&&d_med==0&&d_inf==0&&F==0){
				break;
			}
			printf("La mezcla %c es la que posee menor MPa permitido con unn valor de: %lf", mezcla_menor, r_min);
			printf("La mezcla %c es la que posee mayor MPa permitido con unn valor de: %lf", mezcla_mayor, r_max);		
		}
	}
	return 0;
}

void leer_rango(int*r_inicial, int*r_final){
	printf("Ingrese un valor inicial para el rango: ");
	scanf("%d", r_inicial);
	printf("Ingrese un valor maximo para el rango: ");
	scanf("%d", r_final);
}

int validar_si_esta_en_rango(int res, int r_inicial, int r_final){
	if(res>=r_inicial&&res<=r_final)
		return 1;
	else
		return 0;	
}

double calcular_resistencia_en_MPa(double d_sup, double d_inf, double d_med, double F, char unidad_de_medida){
	double res_en_MPa, area;
	area=PI*(pow(d_sup,2)+pow(d_med,2)+pow(d_inf,2))/12;
	if(unidad_de_medida=='1'){
		area=area*100;
	}
	if(unidad_de_medida=='m'){
		area=area/10000;
	}
	if(unidad_de_medida=='c'){
		area=area;
	}
	res_en_MPa=10*(F/area);
	return res_en_MPa;
}

void hallar_max__min(double*r_max, double*r_min, char mezcla, char unidad_de_medida, double d_sup, double d_med, double d_inf, double F, int r_inicial, int r_final, char *mezcla_min, char *mezcla_max){
	double r;
	int valido;
	r=calcular_resistencia_en_MPa(d_sup, d_inf, d_med, F, unidad_de_medida);
	valido=(validar_si_esta_en_rango(r, r_inicial, r_final));
	if(valido){
		if(r>=(*r_max)){
			(*r_max)=r;
			(*mezcla_max)=mezcla;
		}
		if(r<=(*r_min)){
			(*r_min)=r;
			(*mezcla_min)=mezcla;
		}
	}
}

