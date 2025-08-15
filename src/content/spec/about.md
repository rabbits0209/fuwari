<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>兔兔的个人空间</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }

        :root {
            --primary: #6a5af9;
            --secondary: #d66efd;
            --accent: #ff7aa2;
            --light: #f8f9fa;
            --dark: #2c2c2c;
            --gray: #6c757d;
            --card-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }

        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4edf5 100%);
            color: var(--dark);
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        /* 头部区域 */
        .header {
            text-align: center;
            padding: 30px 0;
            position: relative;
            margin-bottom: 40px;
        }

        .avatar-container {
            position: relative;
            display: inline-block;
            margin-bottom: 20px;
        }

        .avatar {
            width: 180px;
            height: 180px;
            border-radius: 50%;
            object-fit: cover;
            border: 5px solid white;
            box-shadow: var(--card-shadow);
        }

        .status-dot {
            position: absolute;
            width: 25px;
            height: 25px;
            background-color: #2ecc71;
            border-radius: 50%;
            border: 3px solid white;
            bottom: 15px;
            right: 15px;
            animation: pulse 2s infinite;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            font-weight: 800;
        }

        .header p {
            font-size: 1.2rem;
            color: var(--gray);
            max-width: 600px;
            margin: 0 auto;
            font-weight: 300;
        }

        /* 主要内容区域 */
        .main-content {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
        }

        .card {
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: var(--card-shadow);
            transition: var(--transition);
            display: flex;
            flex-direction: column;
            height: 100%;
        }

        .card:hover {
            transform: translateY(-10px);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
        }

        .card-header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 20px;
            display: flex;
            align-items: center;
        }

        .card-header i {
            font-size: 1.5rem;
            margin-right: 12px;
        }

        .card-header h2 {
            font-size: 1.5rem;
            font-weight: 600;
        }

        .card-image {
            height: 200px;
            overflow: hidden;
            position: relative;
        }

        .card-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s ease;
        }

        .card:hover .card-image img {
            transform: scale(1.05);
        }

        .card-body {
            padding: 25px;
            flex-grow: 1;
        }

        .card-body ul {
            list-style-type: none;
        }

        .card-body li {
            padding: 10px 0;
            border-bottom: 1px solid #eee;
            display: flex;
            align-items: flex-start;
        }

        .card-body li:last-child {
            border-bottom: none;
        }

        .card-body li i {
            color: var(--primary);
            margin-right: 12px;
            min-width: 24px;
            text-align: center;
            margin-top: 4px;
        }

        /* 技能标签 */
        .skills {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 15px;
        }

        .skill-tag {
            background: linear-gradient(to right, #e0e7ff, #d1e0fd);
            color: var(--primary);
            padding: 8px 15px;
            border-radius: 50px;
            font-size: 0.9rem;
            font-weight: 500;
        }

        /* 底部区域 */
        .footer {
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: var(--card-shadow);
        }

        .footer h2 {
            text-align: center;
            font-size: 1.8rem;
            color: var(--primary);
            margin-bottom: 30px;
            position: relative;
        }

        .footer h2:after {
            content: '';
            display: block;
            width: 80px;
            height: 4px;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            margin: 10px auto 0;
            border-radius: 2px;
        }

        .social-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
        }

        .social-card {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            border-radius: 15px;
            padding: 25px;
            text-align: center;
            transition: var(--transition);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }

        .social-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            background: linear-gradient(135deg, #ffffff 0%, #f1f3f5 100%);
        }

        .social-icon {
            width: 70px;
            height: 70px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 20px;
            color: white;
            font-size: 30px;
        }

        .social-card h3 {
            font-size: 1.3rem;
            margin-bottom: 10px;
            color: var(--dark);
        }

        .social-card p {
            color: var(--gray);
            font-size: 0.95rem;
        }

        /* 响应式设计 */
        @media (max-width: 768px) {
            .header h1 {
                font-size: 2rem;
            }
            
            .header p {
                font-size: 1rem;
            }
            
            .avatar {
                width: 150px;
                height: 150px;
            }
            
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .social-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .card-image {
                height: 180px;
            }
        }

        @media (max-width: 480px) {
            .header h1 {
                font-size: 1.8rem;
            }
            
            .card-header h2 {
                font-size: 1.3rem;
            }
            
            .card-body {
                padding: 20px;
            }
            
            .social-grid {
                grid-template-columns: 1fr;
            }
            
            .footer {
                padding: 20px;
            }
        }

        /* 动画效果 */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .section-title {
            text-align: center;
            margin: 30px 0;
            position: relative;
        }

        .section-title h2 {
            display: inline-block;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            padding: 0 20px;
            position: relative;
            z-index: 1;
        }

        .section-title:after {
            content: '';
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translateX(-50%);
            width: 80%;
            height: 2px;
            background: linear-gradient(to right, transparent, #ddd, transparent);
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 头部区域 -->
        <header class="header">
            <div class="avatar-container">
                <img src="https://flies.tutublog.eu.org/file/1737866643546_b_be918e46b6b687eb9d93f97cbb321512.jpg" alt="兔兔的头像" class="avatar">
                <div class="status-dot"></div>
            </div>
            <h1>兔兔的个人空间</h1>
            <p>熙熙攘攘，做个人间过客</p>
        </header>

        <!-- 主要内容区域 -->
        <div class="main-content">
            <!-- 个人信息卡片 -->
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-user"></i>
                    <h2>个人信息</h2>
                </div>
                <div class="card-image">
                    <img src="https://t.mwm.moe/ysmp" alt="个人信息背景">
                </div>
                <div class="card-body">
                    <ul>
                        <li>
                            <i class="fas fa-signature"></i>
                            <span>姓名：兔兔</span>
                        </li>
                        <li>
                            <i class="fas fa-map-marker-alt"></i>
                            <span>住址：广东省</span>
                        </li>
                        <li>
                            <i class="fas fa-graduation-cap"></i>
                            <span>学校：不知名的小学校</span>
                        </li>
                        <li>
                            <i class="fas fa-venus-mars"></i>
                            <span>性别：颜控</span>
                        </li>
                        <li>
                            <i class="fas fa-book"></i>
                            <span>年级：二年级</span>
                        </li>
                        <li>
                            <i class="fas fa-star"></i>
                            <span>擅长：人工智能，网站开发</span>
                        </li>
                        <li>
                            <i class="fas fa-envelope"></i>
                            <span>邮箱：1742305143@qq.com</span>
                        </li>
                    </ul>
                </div>
            </div>

            <!-- 教育背景卡片 -->
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-university"></i>
                    <h2>教育背景</h2>
                </div>
                <div class="card-image">
                    <img src="https://t.mwm.moe/ys" alt="教育背景">
                </div>
                <div class="card-body">
                    <ul>
                        <li>
                            <i class="fas fa-school"></i>
                            <span>学校：野鸡大学</span>
                        </li>
                        <li>
                            <i class="fas fa-book-open"></i>
                            <span>主要专业课程：干饭.jpg (101.5分)</span>
                        </li>
                        <li>
                            <i class="fas fa-medal"></i>
                            <span>荣誉奖项：干饭大赛一等奖</span>
                        </li>
                        <li>
                            <i class="fas fa-calendar"></i>
                            <span>入学时间：2023年9月</span>
                        </li>
                    </ul>
                </div>
            </div>

            <!-- 主要项目卡片 -->
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-project-diagram"></i>
                    <h2>主要项目</h2>
                </div>
                <div class="card-image">
                    <img src="https://t.mwm.moe/ysz" alt="项目背景">
                </div>
                <div class="card-body">
                    <ul>
                        <li>
                            <i class="fas fa-blog"></i>
                            <span>Hexo 博客系统开发</span>
                        </li>
                        <li>
                            <i class="fas fa-robot"></i>
                            <span>GitHub Action 自动打包工具</span>
                        </li>
                        <li>
                            <i class="fas fa-exchange-alt"></i>
                            <span>搜狗词库自动转谷歌工具</span>
                        </li>
                        <li>
                            <i class="fas fa-comments"></i>
                            <span>基于 PHP 的即时通讯系统</span>
                        </li>
                        <li>
                            <i class="fas fa-check-circle"></i>
                            <span>基于 GitHub 的自动签到系统</span>
                        </li>
                    </ul>
                </div>
            </div>

            <!-- 技能卡片 -->
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-laptop-code"></i>
                    <h2>技能专长</h2>
                </div>
                <div class="card-image">
                    <img src="https://t.mwm.moe/ycy" alt="技能背景">
                </div>
                <div class="card-body">
                    <ul>
                        <li>
                            <i class="fas fa-language"></i>
                            <span>语言技能：汉语、英语</span>
                        </li>
                        <li>
                            <i class="fas fa-code"></i>
                            <span>专业技能：</span>
                        </li>
                    </ul>
                    <div class="skills">
                        <div class="skill-tag">C++</div>
                        <div class="skill-tag">Java</div>
                        <div class="skill-tag">Python</div>
                        <div class="skill-tag">Linux</div>
                        <div class="skill-tag">HTML/CSS</div>
                        <div class="skill-tag">JavaScript</div>
                        <div class="skill-tag">PHP</div>
                        <div class="skill-tag">Git</div>
                    </div>
                    <ul>
                        <li>
                            <i class="fas fa-utensils"></i>
                            <span>通用技能：干饭（专家级）</span>
                        </li>
                    </ul>
                </div>
            </div>

            <!-- 自我评价卡片 -->
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-star"></i>
                    <h2>自我评价</h2>
                </div>
                <div class="card-image">
                    <img src="https://t.mwm.moe/ysmp" alt="自我评价背景">
                </div>
                <div class="card-body">
                    <ul>
                        <li>
                            <i class="fas fa-brain"></i>
                            <span>思想上乐观开朗，乐于助人，具有团队协作精神及创新意识</span>
                        </li>
                        <li>
                            <i class="fas fa-briefcase"></i>
                            <span>工作上极富责任心与信念感，对待工作认真负责，有较强的组织管理及动手能力</span>
                        </li>
                        <li>
                            <i class="fas fa-heart"></i>
                            <span>生活上积极向上，热爱美食与编程，善于发现生活中的美好</span>
                        </li>
                        <li style="border-top: 2px dashed #6a5af9; margin-top: 15px; padding-top: 15px;">
                            <i class="fas fa-check-double"></i>
                            <strong>总结：人嘎嘎好！</strong>
                        </li>
                    </ul>
                </div>
            </div>

            <!-- 兴趣爱好卡片 -->
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-heart"></i>
                    <h2>兴趣爱好</h2>
                </div>
                <div class="card-image">
                    <img src="https://t.mwm.moe/ys" alt="兴趣爱好背景">
                </div>
                <div class="card-body">
                    <ul>
                        <li>
                            <i class="fas fa-code"></i>
                            <span>编程开发：喜欢探索新技术</span>
                        </li>
                        <li>
                            <i class="fas fa-book"></i>
                            <span>阅读：技术书籍和科幻小说</span>
                        </li>
                        <li>
                            <i class="fas fa-music"></i>
                            <span>音乐：喜欢轻音乐和电子音乐</span>
                        </li>
                        <li>
                            <i class="fas fa-utensils"></i>
                            <span>美食：探索各种美食是我的最爱</span>
                        </li>
                        <li>
                            <i class="fas fa-gamepad"></i>
                            <span>游戏：偶尔玩休闲游戏放松</span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>

        <!-- 底部区域 -->
        <footer class="footer">
            <h2>个人站点与联系方式</h2>
            <div class="social-grid">
                <div class="social-card">
                    <div class="social-icon">
                        <i class="fab fa-github"></i>
                    </div>
                    <h3>GitHub</h3>
                    <p>查看我的开源项目</p>
                </div>
                
                <div class="social-card">
                    <div class="social-icon">
                        <i class="fab fa-blog"></i>
                    </div>
                    <h3>个人博客</h3>
                    <p>阅读我的技术文章</p>
                </div>
                
                <div class="social-card">
                    <div class="social-icon">
                        <i class="fab fa-qq"></i>
                    </div>
                    <h3>QQ</h3>
                    <p>1742305143</p>
                </div>
                
                <div class="social-card">
                    <div class="social-icon">
                        <i class="fas fa-envelope"></i>
                    </div>
                    <h3>邮箱</h3>
                    <p>1742305143@qq.com</p>
                </div>
            </div>
        </footer>
    </div>

    <script>
        // 添加简单的交互效果
        document.addEventListener('DOMContentLoaded', function() {
            const cards = document.querySelectorAll('.card');
            
            cards.forEach(card => {
                card.addEventListener('mouseenter', function() {
                    this.style.transform = 'translateY(-10px)';
                });
                
                card.addEventListener('mouseleave', function() {
                    this.style.transform = 'translateY(0)';
                });
            });
        });
    </script>
</body>
</html>