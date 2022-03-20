#include <stdio.h>
#include<mysql.h>
#include<string.h>
#include<stdlib.h>

int main(int argc, char *argv[]) {
	
	return 0;
}

//devuelve el tablero en el que el Jugador ha obtenido más puntos
//requiere el ID del Jugador como parámetro
void Tablero_Puntos_Maximos(int id_j)
{ MYSQL *conn;
int err;
char consulta[300];
// Estructura especial para almacenar resultados de consultas 
MYSQL_RES *resultado;
MYSQL_ROW row;
conn = mysql_init(NULL);
if (conn==NULL) {
	printf ("Error al crear la conexi??n: %u %s\n", 
			mysql_errno(conn), mysql_error(conn));
	exit (1);
}
conn = mysql_real_connect (conn, "localhost","user", "pass", "personas",0, NULL, 0);
if (conn==NULL) {
	printf ("Error al inicializar la conexi??n: %u %s\n", 
			mysql_errno(conn), mysql_error(conn));
	exit (1);
}
// consulta SQL para obtener una tabla con todos los datos de la base de datos
err=mysql_query (conn, "SELECT * FROM M07_db2.sql");
if (err!=0) {
	printf ("Error al consultar datos de la base %u %s\n",
			mysql_errno(conn), mysql_error(conn));
	exit (1);
}
/*recogemos el resultado de la consulta. El resultado de la
consulta se devuelve en una variable del tipo puntero a
MYSQL_RES tal y como hemos declarado anteriormente.
Se trata de una tabla virtual en memoria que es la copia
de la tabla real en disco.*/
	printf("Este es el escenario en el que has obtenido más puntos\n");
	sprintf(consulta,"SELECT PARTIDA.TABLERO FROM RESULTADOS,JUGADOR,PARTIDA WHERE JUGADOR.ID = '%d'",id_j);
	strcat(consulta, " AND MAX(RESULTADOS.PUNTOS) AS TableroMaximo");
	err=mysql_query (conn, consulta); 
	if (err!=0) {
		printf ("Error al consultar datos de la base %u %s\n",
				mysql_errno(conn), mysql_error(conn));
		exit (1);
	}
	// cerrar la conexion con el servidor MYSQL 
	mysql_close (conn);
	exit(0);
	
}

