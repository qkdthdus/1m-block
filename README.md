www.google.com에 대한 트래픽이 차단되지 않고 연결되는 이유는 다음과 같은 몇 가지 기술적 배경 때문일 수 있습니다. 아래에 구체적으로 설명드릴게요.

🔍 1. Host 헤더가 www.google.com이 아닐 수 있음
HTTP 요청의 Host 헤더는 브라우저나 클라이언트가 설정하는 값인데, Google은 다양한 도메인으로 접근이 가능합니다:

www.google.com

google.com

ipv6.google.com

webcache.googleusercontent.com 등

실제로 www.google.com으로 접속했더라도, 내부 요청에서는 Host: google.com으로 설정되는 경우가 많습니다. 즉, 필터링할 문자열이 "Host: www.google.com"이 아니라 "Host: google.com"일 가능성이 높아요.

➡ 해결 방법: "google.com" 전체를 포함한 필터링을 해보세요.

🔍 2. HTTPS 트래픽은 Host 필터링이 안 됨 (중요)
Google은 기본적으로 HTTPS(즉, 암호화된 요청)를 사용합니다. 예:

arduino
복사
편집
https://www.google.com
이 경우, HTTP 헤더는 암호화되기 때문에 Netfilter Queue 프로그램에서는 Host 필드를 볼 수 없습니다.

🔐 HTTPS 트래픽에서는 Host: 헤더는 TLS Client Hello의 SNI (Server Name Indication) 필드로 전달되며, 암호화된 이후에는 평문 헤더를 볼 수 없습니다.

➡ 해결 방법:

iptables에서 80 포트만 리다이렉트되도록 설정했는지 확인 (HTTP만 감시)

또는 SSL 트래픽을 다루려면 SNI 기반 필터링으로 전환해야 함

🔍 3. Google의 다양한 CDN 및 IP 우회
Google은 글로벌 CDN 및 프록시 서버를 많이 사용하기 때문에, 사용자가 접근하는 실제 호스트가 www.google.com이 아닐 수 있습니다. 다음 중 하나일 수 있어요:

IP 기반 접근 (216.58.200.100 등)

Host 헤더가 www.google.com이 아니고 google.com 혹은 다른 서브도메인일 수 있음
