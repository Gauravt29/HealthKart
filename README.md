# HealthKart
Intern Assignment – HealthKart

1. Introduction 
HealthKart leverages influencer campaigns across platforms such as Instagram, YouTube, and Twitter to promote brands like MuscleBlaze, HKVitals, and Gritzo. Measuring campaign performance and Return on Ad Spend (ROAS) is critical to ensure marketing budgets are allocated efficiently.

This project provides:
~ A structured MySQL database to store influencer campaign data.
~ SQL views to calculate ROI, ROAS, and influencer insights.
~ Integration-ready data for dashboards (Power BI, Streamlit, etc.).
~ Actionable business insights to optimize influencer marketing..

2. Database Design
The system is designed using a relational database model with 4 core tables and foreign key relationships.

 Tables:
1️ influencers: Stores influencer demographic and platform details.

Fields: id, name, category, gender, follower_count, platform

2️ posts: Captures engagement metrics for each influencer post.

Fields: id, influencer_id, platform, date, caption, reach, likes, comments

3️ tracking_data: Tracks campaign performance, orders, and revenue.

Fields: id, source, campaign, influencer_id, user_id, product, date, orders, revenue

4️ payouts: Maintains payout details based on post or order basis.

Fields: id, influencer_id, basis, rate, orders, total_payout

3. Insights from Data
- Top Performer: Priya Sharma (YouTube) generated ₹50,000 revenue with ROAS of 4.0.
- Best ROAS: John Doe (Instagram) – ROAS 4.8 with low payout.
  - Low Performer: Arjun Singh (Twitter) – ROAS < 1, payout exceeds revenue.

**SQL DATACODE**

-- Create Database
CREATE DATABASE healthkart;
USE healthkart;


-- 1. Influencers Table

CREATE TABLE influencers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    category VARCHAR(50),
    gender VARCHAR(10),
    follower_count INT,
    platform VARCHAR(20)
);


-- 2. Posts Table

CREATE TABLE posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    influencer_id INT,
    platform VARCHAR(20),
    date DATE,
    caption TEXT,
    reach INT,
    likes INT,
    comments INT,
    FOREIGN KEY (influencer_id) REFERENCES influencers(id)
);


-- 3. Tracking Data Table

CREATE TABLE tracking_data (
    id INT PRIMARY KEY AUTO_INCREMENT,
    source VARCHAR(50),
    campaign VARCHAR(50),
    influencer_id INT,
    user_id INT,
    product VARCHAR(50),
    date DATE,
    orders INT,
    revenue DECIMAL(10,2),
    FOREIGN KEY (influencer_id) REFERENCES influencers(id)
);


-- 4. Payouts Table

CREATE TABLE payouts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    influencer_id INT,
    basis VARCHAR(20), -- post/order
    rate DECIMAL(10,2),
    orders INT,
    total_payout DECIMAL(10,2),
    FOREIGN KEY (influencer_id) REFERENCES influencers(id)
);


-- Insert Sample Data


INSERT INTO influencers (name, category, gender, follower_count, platform) VALUES
('John Doe', 'Fitness', 'Male', 150000, 'Instagram'),
('Priya Sharma', 'Health', 'Female', 200000, 'YouTube'),
('Arjun Singh', 'Sports', 'Male', 120000, 'Twitter'),
('Meera Iyer', 'Nutrition', 'Female', 180000, 'Instagram'),
('Ravi Patel', 'Fitness', 'Male', 110000, 'YouTube'),
('Ananya Rao', 'Health', 'Female', 175000, 'Instagram'),
('Vikram Nair', 'Bodybuilding', 'Male', 95000, 'Twitter'),
('Sanya Gupta', 'Yoga', 'Female', 140000, 'Instagram'),
('Karan Mehta', 'Supplements', 'Male', 130000, 'YouTube'),
('Neha Kapoor', 'Wellness', 'Female', 160000, 'Instagram');

INSERT INTO posts (influencer_id, platform, date, caption, reach, likes, comments) VALUES
(1, 'Instagram', '2025-07-01', 'MuscleBlaze Protein Promo', 52000, 4500, 320),
(2, 'YouTube', '2025-07-02', 'HKVitals Vitamins', 82000, 5000, 400),
(3, 'Twitter', '2025-07-03', 'Gritzo Energy Drink', 30000, 2000, 100),
(4, 'Instagram', '2025-07-04','Healthy Snacks', 60000, 4700, 350),
(5, 'YouTube', '2025-07-05', 'MB Whey Protein Review', 75000, 4800, 360),
(6, 'Instagram', '2025-07-06','Multivitamin Awareness', 65000, 4300, 310),
(7, 'Twitter', '2025-07-07','Bodybuilding Tips', 28000, 1900, 120),
(8, 'Instagram', '2025-07-08', 'Morning Yoga Routine', 55000, 4200, 300),
(9, 'YouTube', '2025-07-09','Creatine Benefits', 72000, 4600, 340),
(10, 'Instagram', '2025-07-10', 'Wellness Workshop', 58000, 4100, 290),
(1, 'Instagram', '2025-07-11','MuscleBlaze BCAA', 50000, 4000, 280),
(2, 'YouTube', '2025-07-12', 'HKVitals Omega 3', 79000, 5200, 410),
(3, 'Twitter', '2025-07-13', 'Energy Drink Giveaway', 32000, 2100, 150),
(4, 'Instagram', '2025-07-14','Snack Box Review', 61000, 4400, 330),
(5, 'YouTube', '2025-07-15','Whey Protein vs Plant Protein', 78000, 4900, 370);

INSERT INTO tracking_data (source, campaign, influencer_id, user_id, product, date, orders, revenue) VALUES
('Instagram', 'MB_Campaign1', 1, 101, 'MuscleBlaze Protein', '2025-07-05', 120, 24000),
('YouTube', 'HK_Campaign1', 2, 102, 'HKVitals Vitamins', '2025-07-06', 200, 50000),
('Twitter', 'Gritzo_Campaign1', 3, 103, 'Gritzo Energy Drink', '2025-07-07', 80, 16000),
('Instagram', 'Snack_Campaign', 4, 104, 'Healthy Snacks', '2025-07-08', 90, 18000),
('YouTube', 'MB_Campaign2', 5, 105, 'Whey Protein', '2025-07-09', 150, 37500),
('Instagram', 'MultiVit_Campaign', 6, 106, 'Multivitamins', '2025-07-10', 130, 26000),
('Twitter', 'Body_Campaign', 7, 107, 'Bodybuilding Guide', '2025-07-11', 70, 14000),
('Instagram', 'Yoga_Campaign', 8, 108, 'Yoga Kit', '2025-07-12', 100, 20000),
('YouTube', 'Creatine_Campaign', 9, 109, 'Creatine', '2025-07-13', 140, 35000),
('Instagram', 'Wellness_Campaign', 10, 110, 'Wellness Kit', '2025-07-14', 110, 22000),
('Instagram', 'MB_BCAA', 1, 111, 'MuscleBlaze BCAA', '2025-07-15', 100, 20000),
('YouTube', 'Omega_Campaign', 2, 112, 'Omega 3', '2025-07-16', 180, 45000),
('Twitter', 'Energy_Giveaway', 3, 113, 'Energy Drink', '2025-07-17', 60, 12000),
('Instagram', 'Snack_Box', 4, 114, 'Snack Box', '2025-07-18', 95, 19000),
('YouTube', 'Protein_Compare', 5, 115, 'Plant Protein', '2025-07-19', 160, 40000),
('Instagram', 'Yoga_Mat', 8, 116, 'Yoga Mat', '2025-07-20', 85, 17000),
('YouTube', 'Vitamin_Boost', 6, 117, 'Vitamin C', '2025-07-21', 145, 29000),
('Instagram', 'Wellness_Box', 10, 118, 'Wellness Box', '2025-07-22', 105, 21000),
('Instagram', 'Fitness_Starter', 1, 119, 'Starter Kit', '2025-07-23', 115, 23000),
('YouTube', 'Supplement_Stack', 9, 120, 'Supplement Stack', '2025-07-24', 155, 38750);

INSERT INTO payouts (influencer_id, basis, rate, orders, total_payout) VALUES
(1, 'post', 5000, 0, 5000),
(2, 'order', 250, 200, 50000),
(3, 'order', 200, 80, 16000),
(4, 'post', 4500, 0, 4500),
(5, 'order', 250, 150, 37500),
(6, 'order', 200, 130, 26000),
(7, 'order', 200, 70, 14000),
(8, 'post', 4000, 0, 4000),
(9, 'order', 250, 140, 35000),
(10, 'post', 4200, 0, 4200);

-- =====================
-- Create Views
-- =====================

-- Campaign Performance
CREATE VIEW v_campaign_performance AS
SELECT 
    t.campaign,
    SUM(t.orders) AS Total_Orders,
    SUM(t.revenue) AS Total_Revenue,
    COUNT(DISTINCT t.influencer_id) AS Active_Influencers
FROM tracking_data t
GROUP BY t.campaign;

-- ROI & Incremental ROAS
CREATE VIEW v_roi_roas AS
SELECT 
    i.name AS Influencer,
    t.campaign,
    SUM(t.revenue) AS Total_Revenue,
    p.total_payout AS Total_Payout,
    ROUND(SUM(t.revenue) / p.total_payout, 2) AS ROAS
FROM tracking_data t
JOIN influencers i ON t.influencer_id = i.id
JOIN payouts p ON t.influencer_id = p.influencer_id
GROUP BY i.name, t.campaign, p.total_payout;

-- Top Influencers
CREATE VIEW v_top_influencers AS
SELECT 
    i.name AS Influencer,
    i.category,
    i.platform,
    SUM(t.revenue) AS Total_Revenue,
    SUM(t.orders) AS Total_Orders
FROM tracking_data t
JOIN influencers i ON t.influencer_id = i.id
GROUP BY i.name, i.category, i.platform;

-- Poor ROI Influencers
CREATE VIEW v_poor_roi AS
SELECT 
    i.name AS Influencer,
    ROUND(SUM(t.revenue)/p.total_payout, 2) AS ROAS
FROM tracking_data t
JOIN influencers i ON t.influencer_id = i.id
JOIN payouts p ON t.influencer_id = p.influencer_id
GROUP BY i.name, p.total_payout
HAVING ROAS < 1;

-- Payout Tracking
CREATE VIEW v_payout_tracking AS
SELECT 
    i.name AS Influencer,
    p.basis,
    p.rate,
    p.orders,
    p.total_payout,
    SUM(t.revenue) AS Revenue,
    ROUND(SUM(t.revenue)/p.total_payout,2) AS ROAS
FROM payouts p
JOIN influencers i ON p.influencer_id = i.id
LEFT JOIN tracking_data t ON p.influencer_id = t.influencer_id
GROUP BY i.name, p.basis, p.rate, p.orders, p.total_payout;



