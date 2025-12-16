# base-de-datos-I

## crear tablas
USE test;
DROP TABLE IF EXISTS DetallePedido, Pedidos, Productos, Clientes, RegistroVentas;

CREATE TABLE Clientes (
ClienteID VARCHAR(10) PRIMARY KEY,
Nombre VARCHAR(100) NOT NULL,
Direccion VARCHAR(150)
);

CREATE TABLE Productos (
ProductoID VARCHAR(10) PRIMARY KEY,
Nombre VARCHAR(100) NOT NULL,
PrecioUnitario DECIMAL(10,2) NOT NULL
);

CREATE TABLE Pedidos (
PedidoID VARCHAR(10) PRIMARY KEY,
FechaPedido DATE NOT NULL,
ClienteID VARCHAR(10),
FOREIGN KEY (ClienteID) REFERENCES Clientes(ClienteID)
);

CREATE TABLE DetallePedido (
PedidoID VARCHAR(10),
ProductoID VARCHAR(10),
Cantidad INT NOT NULL,
PrecioUnitario DECIMAL(10,2) NOT NULL,
PRIMARY KEY (PedidoID, ProductoID),
FOREIGN KEY (PedidoID) REFERENCES Pedidos(PedidoID),
FOREIGN KEY (ProductoID) REFERENCES Productos(ProductoID)
);



-- Insertar Clientes
INSERT INTO Clientes (ClienteID, Nombre, Direccion) VALUES
('C01', 'Ana Perez', 'Av. Los Olivos 123'),
('C02', 'Juan Ruiz', 'Jr. La Luna #1');

-- Insertar Productos
INSERT INTO Productos (ProductoID, Nombre, PrecioUnitario) VALUES
('PR01', 'Pera', 2.0),
('PR02', 'Manzana', 1.50),
('PR03', 'Manzana', 1.50),
('PR04', 'Uva', 3.00);

-- Insertar Pedidos
INSERT INTO Pedidos (PedidoID, FechaPedido, ClienteID) VALUES
('P001', '2023-09-10', 'C01'),
('P002', '2023-09-10', 'C02'),
('P003', '2023-09-11', 'C01');

-- Insertar DetallePedido
INSERT INTO DetallePedido (PedidoID, ProductoID, Cantidad, PrecioUnitario) VALUES
('P001', 'PR01', 3.5, 2.00),
('P002', 'PR02', 3.5, 1.50),
('P002', 'PR03', 1, 1.50),
('P003', 'PR04', 2, 3.00);

## Consultas

¿Cuál es el nombre y el salario de todos los empleados? 
• SELECT ENAME, SAL FROM EMP;
¿Qué empleados trabajan en el departamento 10? 
• SELECT ENAME FROM EMP WHERE DEPTNO =10;
¿Cuál es el nombre de los departamentos y sus ubicaciones?
• SELECT DNAME, LOC FROM DEPT;
¿Cuáles son los empleados que tienen un salario mayor a 2000?
• SELECT ENAME, SAL FROM EMP WHERE SAL >2000;
¿Qué empleados tienen el cargo de ‘MANAGER’?
• SELECT ENAME FROM EMP WHERE JOB ='MANAGER';
¿Qué empleados no tienen comisión?
• SELECT ENAME FROM EMP WHERE COMM IS NULL;
¿Cuál es el nombre del empleado y su fecha de contratación?
• SELECT ENAME, HIREDATE FROM EMP;
¿Cuántos empleados hay en cada departamento?
• SELECT DEPTNO, COUNT(*) AS NUM_EMPLEADOS FROM EMP GROUP BY DEPTNO;
¿Qué empleados tienen un salario entre 1000 y 3000?
• SELECT ENAME, SAL FROM EMP WHERE SAL BETWEEN 1000 AND 3000;
¿Qué nombre y ubicación tiene el departamento 30?
• SELECT DNAME, LOC FROM DEPT WHERE DEPTNO = 30;
--
¿Cuál es el salario promedio por departamento?
• SELECT DEPTNO, AVG(SAL) AS PROMEDIO_SALARIO FROM EMP GROUP BY DEPTNO;
¿Qué empleados ganan más que el promedio general de salarios?
• SELECT ENAME, SAL FROM EMP WHERE SAL > (SELECT AVG(SAL) FROM EMP);
¿Cuáles son los nombres de empleados y el nombre de su departamento?
• SELECT E.ENAME, D.DNAME FROM EMP E JOIN DEPT D ON E.DEPTNO = D.DEPTNO;
¿Qué empleados fueron contratados antes de 1982?
• SELECT ENAME, HIREDATE FROM EMP WHERE HIREDATE < TO_DATE('01-01-1982','DD-MM-YYYY');
¿Qué departamentos no tienen empleados asignados?
• SELECT DNAME FROM DEPT WHERE DEPTNO NOT IN (SELECT DISTINCT DEPTNO FROM EMP);
¿Cuál es el segundo salario más alto entre los empleados?
• SELECT MAX(SAL) FROM EMP WHERE SAL < (SELECT MAX(SAL) FROM EMP);
¿Qué empleados tienen el mismo cargo que 'JONES'?
• SELECT ENAME FROM EMP WHERE JOB = (SELECT JOB FROM EMP WHERE ENAME = 'JONES');
¿Qué empleados trabajan en la misma ubicación que el departamento 10?
• SELECT E.ENAME FROM EMP E JOIN DEPT D ON E.DEPTNO = D.DEPTNO WHERE D.LOC = (SELECT LOC FROM DEPT WHERE DEPTNO = 10);
¿Cuáles son los empleados con comisión mayor a su salario?
SELECT ENAME, SAL, COMM FROM EMP WHERE COMM > SAL;
¿Qué empleados tienen un jefe y en qué departamento trabajan?
SELECT ENAME, MGR, DEPTNO FROM EMP WHERE MGR IS NOT NULL;
---
¿Qué empleados tienen el salario más alto en cada departamento?
• SELECT ENAME, SAL, DEPTNO • FROM EMP E • WHERE SAL = (SELECT MAX(SAL) FROM EMP WHERE DEPTNO = E.DEPTNO);
¿Cuáles son los empleados que no son jefes de nadie?
• SELECT ENAME FROM EMP • WHERE EMPNO NOT IN (SELECT DISTINCT MGR FROM EMP WHERE MGR IS NOT NULL);
¿Qué empleados tienen el mismo salario que al menos otro empleado?
SELECT ENAME, SAL FROM EMP • WHERE SAL IN (SELECT SAL FROM EMP GROUP BY SAL HAVING COUNT(*) > 1);
¿Qué empleados tienen un salario superior al promedio de su departamento?
• SELECT ENAME, SAL, DEPTNO FROM EMP E • WHERE SAL > (SELECT AVG(SAL) FROM EMP WHERE DEPTNO = E.DEPTNO);
¿Cuál es el nombre del empleado y su jefe?
• SELECT E.ENAME AS EMPLEADO, M.ENAME AS JEFE FROM EMP E LEFT JOIN EMP M ON E.MGR = M.EMPNO;
¿Cuál es el total de salarios y comisiones por departamento?
• SELECT DEPTNO, SUM(SAL + NVL(COMM, 0)) AS TOTAL_COMPENSACION • FROM EMP • GROUP BY DEPTNO;
¿Qué empleados tienen un salario mayor que el de su jefe?
• SELECT E.ENAME FROM EMP E • JOIN EMP M ON E.MGR = M.EMPNO • WHERE E.SAL > M.SAL;
¿Cuáles son los departamentos con más de 3 empleados?
• SELECT DEPTNO FROM EMP • GROUP BY DEPTNO • HAVING COUNT(*) > 3;
¿Qué empleados han sido contratados en el mes de diciembre?
• SELECT ENAME, HIREDATE FROM EMP • WHERE TO_CHAR(HIREDATE, 'MM') = '12';
¿Qué empleados tienen al menos 2 colegas con el mismo cargo?
• SELECT ENAME, JOB FROM EMP • WHERE JOB IN ( • SELECT JOB FROM EMP GROUP BY JOB HAVING COUNT(*) >= 3 • );
Salarios mayores al promedio del departamento
• SELECT e.empno, e.ename, e.sal, e.deptno • FROM emp e • WHERE e.sal > ( • SELECT AVG(sal) • FROM emp • WHERE deptno = e.deptno • );
Empleados con salario duplicado
• SELECT empno, ename, sal FROM emp WHERE sal IN ( SELECT sal FROM emp GROUP BY sal HAVING COUNT(*) > 1 );
