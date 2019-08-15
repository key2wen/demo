package com.demo.ssdemo.core.filter;

import com.demo.ssdemo.sys.entity.Resource;
import com.demo.ssdemo.sys.entity.Role;
import com.demo.ssdemo.sys.service.RoleService;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.web.access.WebInvocationPrivilegeEvaluator;
import org.springframework.stereotype.Component;

import java.util.List;

/**
 * @Author OZY
 * @Date 2019/07/19 20:38
 * @Description 标签权限认证
 * @Version V1.0
 **/
@Component
public class SecTagPrivilegeEvaluator implements WebInvocationPrivilegeEvaluator {

    @javax.annotation.Resource
    private RoleService roleService;

    @Override
    public boolean isAllowed(String uri, Authentication authentication) {
        return true;
    }

    /**
     * 判断是否有该资源权限
     * @param contextPath
     * @param uri
     * @param method
     * @param authentication
     * @return
     */
    @Override
    public boolean isAllowed(String contextPath, String uri, String method, Authentication authentication) {

        List<Role> roleList = roleService.findAll();


        //判断该角色是否存在访问该资源的权限
        //默认每个用户对应一个角色，所以这里就直接获取
        GrantedAuthority grantedAuthority = authentication.getAuthorities().iterator().next();
        //循环角色获取当前角色
        for (Role role : roleList) {
            if (role.getRoleKey().equals(grantedAuthority.getAuthority())) {
                //循环当前角色的资源
                for (Resource resource : role.getResources()) {
                    if (uri.equals(resource.getUrl())) {
                        //如果存在则证明有权限访问
                        return true;
                    }
                }
            }

        }
        //无权限则返回false
        return false;
    }

}
