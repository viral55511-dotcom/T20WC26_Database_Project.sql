# 🏏 T20WC26 Database Project

# T20WC26_Database_Project.sql


A SQL database project designed to store and analyze match and player statistics of the T20 World Cup.

## 🚀 Features

* Stores teams, players, matches, and stats
* Tracks batting, bowling, and fielding performance
* Supports analytical SQL queries
* Clean relational database design

## 🛠 Technologies Used

* SQL
* Database Design
* Data Analytics Concepts

## 📊 Sample Queries

* Top run scorer
* Top wicket taker
* Highest strike rate

## 📁 Project Structure

* database.sql — full SQL script
* README.md — project documentation

## 🎯 Purpose

This project demonstrates database design and SQL skills for real-world sports analytics.

SQL database project for T20 World Cup
-- =====================================================
-- Project: T20WC26 Database
-- Purpose: Store stats and information of T20 World Cup 2026
-- Author: [Your Name]
-- =====================================================

-- Create the database
CREATE DATABASE T20WC26;
-- Select the database to work on
USE T20WC26;

-- Stores all participating teams
CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(50) NOT NULL
);


-- Stores player information
CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(50),
    team_id INT,

    -- Link player to team
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
 );

-- Stores stadium information
CREATE TABLE Venues (
    venue_id INT AUTO_INCREMENT PRIMARY KEY,
    stadium_name VARCHAR(100),
    city VARCHAR(50),
    country VARCHAR(50)
);

-- Stores match details
CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team1_id INT,
    team2_id INT,
    venue_id INT,
    winner_team_id INT,
    result VARCHAR(100),

    FOREIGN KEY (team1_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team2_id) REFERENCES Teams(team_id),
    FOREIGN KEY (venue_id) REFERENCES Venues(venue_id),
    FOREIGN KEY (winner_team_id) REFERENCES Teams(team_id)
);

-- Stores player performance per match
CREATE TABLE PlayerMatchStats (
    stat_id INT AUTO_INCREMENT PRIMARY KEY,
    match_id INT,
    player_id INT,

    -- Batting stats
    runs INT DEFAULT 0,
    balls_faced INT DEFAULT 0,

    -- Bowling stats
    balls_bowled INT DEFAULT 0,
    wickets INT DEFAULT 0,

    -- Fielding stats
    catches INT DEFAULT 0,
    run_outs INT DEFAULT 0,
    stumpings INT DEFAULT 0,

    FOREIGN KEY (match_id) REFERENCES Matches(match_id),
    FOREIGN KEY (player_id) REFERENCES Players(player_id)
);

INSERT INTO Teams (team_name) VALUES
('India'),
('USA'),
('West Indies'),
('Canada'),
('Australia');

INSERT INTO Venues (stadium_name, city, country) VALUES
('Nassau Stadium', 'New York', 'USA'),
('Grand Prairie Stadium', 'Texas', 'USA');

INSERT INTO Players (player_name, team_id) VALUES
('Virat Kohli', 1),
('Rohit Sharma', 1),
('Aaron Jones', 2),
('Nicholas Pooran', 3),
('David Warner', 5);

INSERT INTO Matches
(match_date, team1_id, team2_id, venue_id, winner_team_id, result)
VALUES
('2024-06-01', 1, 2, 1, 1, 'India won by 7 wickets'),
('2024-06-02', 3, 4, 2, 3, 'West Indies won by 5 runs'),
('2024-06-03', 1, 3, 1, 1, 'India won by 6 wickets'),
('2024-06-04', 2, 4, 2, 2, 'USA won by 10 runs'),
('2024-06-05', 5, 1, 1, 5, 'Australia won by 4 wickets');

INSERT INTO PlayerMatchStats
(match_id, player_id, runs, balls_faced, balls_bowled, wickets, catches)
VALUES
(1, 1, 55, 35, 0, 0, 1),
(1, 2, 40, 28, 6, 1, 0),
(2, 4, 60, 32, 0, 0, 1),
(3, 1, 70, 40, 0, 0, 0),
(5, 5, 45, 25, 12, 2, 1);

-- Find player with highest total runs
SELECT P.player_name, SUM(S.runs) AS total_runs
FROM PlayerMatchStats S
JOIN Players P ON S.player_id = P.player_id
GROUP BY P.player_name
ORDER BY total_runs DESC
LIMIT 1;

-- Find player with most wickets
SELECT P.player_name, SUM(S.wickets) AS total_wickets
FROM PlayerMatchStats S
JOIN Players P ON S.player_id = P.player_id
GROUP BY P.player_name
ORDER BY total_wickets DESC
LIMIT 1;

-- Strike rate = (runs / balls) * 100
SELECT P.player_name,
SUM(S.runs)*100 / SUM(S.balls_faced) AS strike_rate
FROM PlayerMatchStats S
JOIN Players P ON S.player_id = P.player_id
GROUP BY P.player_name
ORDER BY strike_rate DESC
LIMIT 1;

-- Bowling strike rate = balls bowled / wickets
SELECT P.player_name,
SUM(S.balls_bowled) / SUM(S.wickets) AS bowling_sr
FROM PlayerMatchStats S
JOIN Players P ON S.player_id = P.player_id
WHERE S.wickets > 0
GROUP BY P.player_name
ORDER BY bowling_sr ASC
LIMIT 1;

-- Remove all old tournament data
TRUNCATE TABLE PlayerMatchStats;
TRUNCATE TABLE Matches;
TRUNCATE TABLE Players;
TRUNCATE TABLE Teams;
TRUNCATE TABLE Venues;







