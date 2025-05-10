# Finals Task 5. Using Stored PRocedures Views and Function

CREATE DATABASE  IF NOT EXISTS `inventory` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci */;
USE `inventory`;
-- MySQL dump 10.13  Distrib 8.0.41, for Win64 (x86_64)
--
-- Host: 127.0.0.1    Database: inventory
-- ------------------------------------------------------
-- Server version	5.5.5-10.4.28-MariaDB

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!50503 SET NAMES utf8 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

--
-- Temporary view structure for view `productnames`
--

DROP TABLE IF EXISTS `productnames`;
/*!50001 DROP VIEW IF EXISTS `productnames`*/;
SET @saved_cs_client     = @@character_set_client;
/*!50503 SET character_set_client = utf8mb4 */;
/*!50001 CREATE VIEW `productnames` AS SELECT 
 1 AS `ProductName`,
 1 AS `VendorName`,
 1 AS `P_INDATE`*/;
SET character_set_client = @saved_cs_client;

--
-- Temporary view structure for view `productrange`
--

DROP TABLE IF EXISTS `productrange`;
/*!50001 DROP VIEW IF EXISTS `productrange`*/;
SET @saved_cs_client     = @@character_set_client;
/*!50503 SET character_set_client = utf8mb4 */;
/*!50001 CREATE VIEW `productrange` AS SELECT 
 1 AS `P_CODE`,
 1 AS `P_DESCRIPT`,
 1 AS `P_INDATE`,
 1 AS `P_ONHAND`,
 1 AS `P_MIN`,
 1 AS `P_PRICE`,
 1 AS `P_DISCOUNT`,
 1 AS `V_CODE`*/;
SET character_set_client = @saved_cs_client;

--
-- Table structure for table `products`
--

DROP TABLE IF EXISTS `products`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `products` (
  `P_CODE` varchar(10) NOT NULL,
  `P_DESCRIPT` varchar(35) NOT NULL,
  `P_INDATE` date NOT NULL,
  `P_ONHAND` int(11) NOT NULL,
  `P_MIN` int(11) NOT NULL,
  `P_PRICE` decimal(8,2) NOT NULL,
  `P_DISCOUNT` decimal(5,2) NOT NULL,
  `V_CODE` int(11) DEFAULT NULL,
  PRIMARY KEY (`P_CODE`),
  KEY `FK_VCode` (`V_CODE`),
  CONSTRAINT `FK_VCode` FOREIGN KEY (`V_CODE`) REFERENCES `vendors` (`V_CODE`) ON DELETE NO ACTION ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `products`
--

LOCK TABLES `products` WRITE;
/*!40000 ALTER TABLE `products` DISABLE KEYS */;
INSERT INTO `products` VALUES ('111111','PAKO','2002-01-17',10,2,100.00,0.00,21227),('11QER/31','SHAMOO','2001-12-03',8,5,109.99,0.00,25595),('13-Q2/P2','7.25-in pwr,saw blade','2002-01-13',32,15,14.99,0.05,21344),('14-Q1/L3','9.00-in pwr, saw blade','2002-01-13',18,12,17.49,0.00,21344),('1546-qq2','Hrd. cloth,1/4-in.2x50','2001-12-15',15,8,39.95,0.00,23119),('1558-QW1','Hrd. cloth, 1/2-in., 3x50','2002-01-15',23,5,43.99,0.00,23119),('2232/QTY','B&D Jigsaw, 12-in. blade','2001-12-30',8,5,109.92,0.05,24288),('2232/QWE','B&D Jigsaw, 8-in. blade','2002-01-24',6,5,99.87,0.05,24288),('2238/QPD','B&D Cordless Drill,12-in. blade','2001-11-20',12,5,38.95,0.05,25595),('23109-HB','Claw Hammer','2002-01-20',23,10,5.95,0.10,21225),('54778-2T','Rat tail-file, 1/8-in fine','2002-01-15',43,20,4.99,0.00,21344),('89-WRE-Q','Hitcut chain saw, 16in.','2001-12-07',11,5,256.99,0.05,24288),('SM-18277','1.25-in. metal screw, 50','2001-12-29',172,75,6.99,0.00,21225),('SW-23116','2.5-in. wd.screw, 50','2001-12-24',237,100,8.45,0.00,21231),('WR3/TT3','Steel Matting','2001-01-17',18,5,119.95,0.10,25595);
/*!40000 ALTER TABLE `products` ENABLE KEYS */;
UNLOCK TABLES;

--
-- Temporary view structure for view `total_price`
--

DROP TABLE IF EXISTS `total_price`;
/*!50001 DROP VIEW IF EXISTS `total_price`*/;
SET @saved_cs_client     = @@character_set_client;
/*!50503 SET character_set_client = utf8mb4 */;
/*!50001 CREATE VIEW `total_price` AS SELECT 
 1 AS `P_CODE`,
 1 AS `P_DESCRIPT`,
 1 AS `P_ONHAND`,
 1 AS `P_PRICE`,
 1 AS `V_CODE`,
 1 AS `TOTAL_PRICE`*/;
SET character_set_client = @saved_cs_client;

--
-- Table structure for table `vendors`
--

DROP TABLE IF EXISTS `vendors`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `vendors` (
  `V_CODE` int(11) NOT NULL,
  `V_NAME` varchar(30) NOT NULL,
  `V_CONTACT` varchar(30) NOT NULL,
  `V_AREACODE` int(11) DEFAULT NULL,
  `V_PHONE` varchar(12) NOT NULL,
  `V_STATE` varchar(3) NOT NULL,
  `V_ORDER` varchar(1) DEFAULT NULL,
  PRIMARY KEY (`V_CODE`),
  KEY `FK_VAreaCode` (`V_AREACODE`),
  CONSTRAINT `FK_VAreaCode` FOREIGN KEY (`V_AREACODE`) REFERENCES `vendorsacode` (`V_AREACODE`) ON DELETE NO ACTION ON UPDATE CASCADE,
  CONSTRAINT `CHK_VORDER` CHECK (`V_ORDER` in ('Y','N'))
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `vendors`
--

LOCK TABLES `vendors` WRITE;
/*!40000 ALTER TABLE `vendors` DISABLE KEYS */;
INSERT INTO `vendors` VALUES (21225,'Bryson,Inc.','Smithson',615,'223-3234','TN','Y'),(21226,'SuperLoo,Inc.','Flushing',904,'215-8995','FL','N'),(21227,'Bryson,Inc.','RASEL',615,'223-3235','TN','Y'),(21231,'D&E Supply','Singh',615,'228-3245','TN','Y'),(21344,'Gomez Bros.','Ortega',615,'889-2546','KY','N'),(22567,'Dome Supply','Smith',901,'678-1419','GA','N'),(23119,'Randsets Ltd.','Anderson',901,'678-3998','GA','Y'),(24004,'Brackman','Browning',615,'228-1410','TN','N'),(24288,'ORDVA,Inc.','Hakford',615,'898-1234','TN','Y'),(25443,'B&K Inc.','Smith',904,'227-0093','FL','N'),(25501,'Damal Supplies','Smythe',615,'890-3529','TN','N'),(25595,'Rubicon Sis.','Orton',904,'456-0092','FL','Y');
/*!40000 ALTER TABLE `vendors` ENABLE KEYS */;
UNLOCK TABLES;

--
-- Table structure for table `vendorsacode`
--

DROP TABLE IF EXISTS `vendorsacode`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `vendorsacode` (
  `V_AREACODE` int(11) NOT NULL,
  `V_AREACODENAME` varchar(15) NOT NULL,
  PRIMARY KEY (`V_AREACODE`),
  UNIQUE KEY `UC_AREACODENAME` (`V_AREACODENAME`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `vendorsacode`
--

LOCK TABLES `vendorsacode` WRITE;
/*!40000 ALTER TABLE `vendorsacode` DISABLE KEYS */;
INSERT INTO `vendorsacode` VALUES (615,'AC'),(901,'MAG'),(904,'SF');
/*!40000 ALTER TABLE `vendorsacode` ENABLE KEYS */;
UNLOCK TABLES;

--
-- Final view structure for view `productnames`
--

/*!50001 DROP VIEW IF EXISTS `productnames`*/;
/*!50001 SET @saved_cs_client          = @@character_set_client */;
/*!50001 SET @saved_cs_results         = @@character_set_results */;
/*!50001 SET @saved_col_connection     = @@collation_connection */;
/*!50001 SET character_set_client      = utf8mb4 */;
/*!50001 SET character_set_results     = utf8mb4 */;
/*!50001 SET collation_connection      = utf8mb4_general_ci */;
/*!50001 CREATE ALGORITHM=UNDEFINED */
/*!50013 DEFINER=`root`@`localhost` SQL SECURITY DEFINER */
/*!50001 VIEW `productnames` AS select `p`.`P_DESCRIPT` AS `ProductName`,`v`.`V_NAME` AS `VendorName`,`p`.`P_INDATE` AS `P_INDATE` from (`products` `p` join `vendors` `v` on(`p`.`V_CODE` = `v`.`V_CODE`)) where `p`.`P_INDATE` >= '2001-01-01' */;
/*!50001 SET character_set_client      = @saved_cs_client */;
/*!50001 SET character_set_results     = @saved_cs_results */;
/*!50001 SET collation_connection      = @saved_col_connection */;

--
-- Final view structure for view `productrange`
--

/*!50001 DROP VIEW IF EXISTS `productrange`*/;
/*!50001 SET @saved_cs_client          = @@character_set_client */;
/*!50001 SET @saved_cs_results         = @@character_set_results */;
/*!50001 SET @saved_col_connection     = @@collation_connection */;
/*!50001 SET character_set_client      = utf8mb4 */;
/*!50001 SET character_set_results     = utf8mb4 */;
/*!50001 SET collation_connection      = utf8mb4_general_ci */;
/*!50001 CREATE ALGORITHM=UNDEFINED */
/*!50013 DEFINER=`root`@`localhost` SQL SECURITY DEFINER */
/*!50001 VIEW `productrange` AS select `products`.`P_CODE` AS `P_CODE`,`products`.`P_DESCRIPT` AS `P_DESCRIPT`,`products`.`P_INDATE` AS `P_INDATE`,`products`.`P_ONHAND` AS `P_ONHAND`,`products`.`P_MIN` AS `P_MIN`,`products`.`P_PRICE` AS `P_PRICE`,`products`.`P_DISCOUNT` AS `P_DISCOUNT`,`products`.`V_CODE` AS `V_CODE` from `products` where `products`.`P_PRICE` between 100 and 150 */;
/*!50001 SET character_set_client      = @saved_cs_client */;
/*!50001 SET character_set_results     = @saved_cs_results */;
/*!50001 SET collation_connection      = @saved_col_connection */;

--
-- Final view structure for view `total_price`
--

/*!50001 DROP VIEW IF EXISTS `total_price`*/;
/*!50001 SET @saved_cs_client          = @@character_set_client */;
/*!50001 SET @saved_cs_results         = @@character_set_results */;
/*!50001 SET @saved_col_connection     = @@collation_connection */;
/*!50001 SET character_set_client      = utf8mb4 */;
/*!50001 SET character_set_results     = utf8mb4 */;
/*!50001 SET collation_connection      = utf8mb4_general_ci */;
/*!50001 CREATE ALGORITHM=UNDEFINED */
/*!50013 DEFINER=`root`@`localhost` SQL SECURITY DEFINER */
/*!50001 VIEW `total_price` AS select `products`.`P_CODE` AS `P_CODE`,`products`.`P_DESCRIPT` AS `P_DESCRIPT`,`products`.`P_ONHAND` AS `P_ONHAND`,`products`.`P_PRICE` AS `P_PRICE`,`products`.`V_CODE` AS `V_CODE`,`products`.`P_ONHAND` * `products`.`P_PRICE` AS `TOTAL_PRICE` from `products` where `products`.`V_CODE` in (21344,23119,24288) */;
/*!50001 SET character_set_client      = @saved_cs_client */;
/*!50001 SET character_set_results     = @saved_cs_results */;
/*!50001 SET collation_connection      = @saved_col_connection */;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

-- Dump completed on 2025-05-10 12:24:01

![screenshot](image/HAKDOG.png)
