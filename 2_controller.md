## 컨트롤러 설치 과정

- **전체 레파지토리 리스트 확인**

  ```
  yum repolist
  ```

- **전체 OS , Kernel을 포함한 업데이트**

  ```
  yum update -y
  ```



- **CentOS 서비스 최적화**

  ```
  # systemctl stop firewalld
  # systemctl disable firewalld
  # systemctl disable NetworkManager
  # systemctl stop NetworkManager
  ```



- **보안매커니즘**

  - 방화벽

  - SE Linux 

    -  각각의 리소스와 주체(사용자, 프로세스)의 lable이 설정되어서 사용자가 디렉토리나 파일 포트에 접근할 때 lable이 일치해야 접근 가능
    - iso를 설치하면 기본적으로 enable이 되어 있음
    - vi /etc/selinux/config 에서 설정 가능

    ```
    # This file controls the state of SELinux on the system.
    # SELINUX= can take one of these three values:
    #     enforcing - SELinux security policy is enforced.
    #     permissive - SELinux prints warnings instead of enforcing.
    #     disabled - No SELinux policy is loaded.
    SELINUX=disabled
    # SELINUXTYPE= can take one of three two values:
    #     targeted - Targeted processes are protected,
    #     minimum - Modification of targeted policy. Only selected processes are protected.
    #     mls - Multi Level Security protection.
    SELINUXTYPE=targeted
    
    ```

    

- **NTP 서버 구성**

```
# yum install chrony -y
# vi /etc/chrony.conf
	/////설정/////
	# Use public servers from the pool.ntp.org project.
	# Please consider joining the pool (http://www.pool.ntp.org/join.html).
	#server 0.centos.pool.ntp.org iburst
	#server 1.centos.pool.ntp.org iburst
	#server 2.centos.pool.ntp.org iburst
	server 3.centos.pool.ntp.org iburst
	server 2.kr.pool.ntp.org.iburst
	server 127.127.1.0

	allow 10.0.0.0/24
	////설정////////

# yum install -y ntpdate
# npdate time.nuri.net
// 기존의 서버와 클라이언트의 시간차 갭을 없애주기 위해 타임 설정
# systemctl start chronyd
# systemctl enable chronyd
# chronyc sources
	210 Number of sources = 2
	MS Name/IP address         Stratum Poll Reach LastRx Last sample               
	===============================================================================
	^? 127.127.1.0                   0   6     0     -     +0ns[   +0ns] +/-    0ns
	^? dadns.cdnetworks.co.kr        2   6     3     1   +791us[ +791us] +/-   41ms
	//현재 자신이 서버이자 클라이언트인 상태
```



- **openstack repository 등록**

  ```
  # yum install -y centos-release-openstack-rocky
  # yum repolist
  # yum upgrade -y
  
  ```

  

- **Packstack 설치**

```
# yum install -y openstack-packstack*
# packstack --gen-answer-file=/root/openstack.txt
# cp /root/openstack.txt /root/openstack.orig
# vi /root/openstack.txt
	//수정//
	326 CONFIG_KEYSTONE_ADMIN_PW=abc123
	1185 CONFIG_PROVISION_DEMO=n
	11 CONFIG_DEFAULT_PASSWORD=abc123
	46 CONFIG_CEILOMETER_INSTALL=n
	50 CONFIG_AODH_INSTALL=n
	873 CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:ens33
	//수정//
	

```
