# 1. 환경 확인사항
* agentconfig.xml 진짜 사용하는 파일인지 확인할 것. -> 샘플파일 주는곳 많으니깐 실제 사용하는거 달라해야함.
* nets.sso.agent.dll도 고객사한테 받아와야 함.
* 고객사 nets 담당자에게 icm 도메인이 등록되어있는지 물어봐야함 -> 없다면 icm도메인 등록 해달라고 요청해야함.
* SLO 방식에선 iis에서 http://icm주소/ssoproc/sso_run.aspx로 다이렉트로 쏘게 하지만 nets 이용시에는
* http://icm주소/ssoproc/Default.aspx로 접근하게 해야함.
* 걔네가 주는 파일들 aspx파일들  (aspx.cs파일 말고) 최상단에 보면
```html
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="_Default" %>
```
라고 있음. CodeFile로 되어있으면 CodeBehind로 바꿔줘야 함. 젠킨스 빌드되면 'cs'가 사라져서 파일명으롷 하면 못찾음.

# 2. 소스 확인사항
* Default.aspx파일 보면 SSO_ID넣어주는거 없을수도 있음. -> * (흥국생명 SSO 참고)
```
// 인증 상태별 처리
if (authstatus == AuthStatus.SSOSuccess)
{
    // ---------------------------------------------------------------------
    // 인증 상태: 인증 성공
    // - 인증 토큰(쿠키) 존재하고, 토큰 형식에 맞고, SSO 정책 체크 결과 유효함.
    // ---------------------------------------------------------------------

    // 사용자 아이디 추출
    _logger.Debug("sso성공 로그");
    userId = auth.UserID;

    Session["SSO_ID"] = userId; // 이거 넣어줘야  SSO_ID 받아옴.
    Response.Redirect("sso_run.aspx", false);
}
```
