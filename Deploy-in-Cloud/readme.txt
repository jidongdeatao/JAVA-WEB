项目部署云服务器

  1、云服务器购买与域名（申请，备案，配置）
    选取的是阿里云ECS，操作系统镜像CentOS，域名在阿里云万网购买,备案也会比较快
    安全组起到与防火墙相同作用，为了安全一定要配置
    如果网络不通，先检查iptables，再检查安全组
    域名解析 使用A字段就可以了，比较简单，不再赘述
    
  2、源配置
    使用了阿里云源配置官网（http://mirrors.aliyun.com/)
    CentOS源配置说明：http://mirrors.aliyun.com/help/centos
    源配置步骤：
      1）备份
        命令：sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
        对源进行重命名
      2）下载CentOS-Base.repo到/etc/yum.repos.d
        wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
      3）源配置步骤
        运行yum makecache生成缓存
  3、配置线上环境
    JDK、Tomcat、Maven、Vsftpd、Nginx、文件服务器、MySQL、Git、iptables
  4、自动化发布
    使用Shell脚本自动化发布
  5、线上验证
    前台：
    后台：
    图片服务器
    前端服务器：
