package com.demo.ssdemo.core.permission;

import com.demo.ssdemo.sys.entity.Resource;
import com.demo.ssdemo.sys.entity.Role;
import com.demo.ssdemo.sys.service.RoleService;
import org.springframework.security.access.ConfigAttribute;
import org.springframework.security.access.SecurityConfig;
import org.springframework.security.web.FilterInvocation;
import org.springframework.security.web.access.intercept.FilterInvocationSecurityMetadataSource;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;
import org.springframework.stereotype.Component;

import java.util.*;

/**
 * @Author OZY
 * @Date 2019/07/16 09:08
 * @Description 权限资源管理器
 * @Version V1.0
 **/
@Component
public class WebSecurityMetadataSource implements FilterInvocationSecurityMetadataSource {

    @javax.annotation.Resource
    private RoleService roleService;

    /**
     * 判断用户请求的url是否在权限表中，如果在权限表中，则返回给decide决策
     * 简单的说，就是每当需要验证权限的时候，都需要从改方法中加载权限，如存在该权限则返回给decide去判断，不存在权限则直接无法访问
     * 该方法不做权限判断
     * @param object 用户请求基本信息
     * @return
     * @throws IllegalArgumentException
     */
    @Override
    @SuppressWarnings("unchecked")
    public Collection<ConfigAttribute> getAttributes(Object object) throws IllegalArgumentException {

        List<Role> roleList = roleService.findAll();

        //转换FilterInvocation类,里面存储了用户请求的基本信息,如request,response等
        FilterInvocation fi = (FilterInvocation) object;

        List<ConfigAttribute> configAttributeList = new ArrayList<>();
        for (Role role : roleList) {
            for (Resource resource : role.getResources()) {
                if (new AntPathRequestMatcher(resource.getUrl()).matches(fi.getRequest())) {
                    //AntPathRequestMatcher为Url匹配器，方法matches如果两Url匹配(相同)则会返回true
                    configAttributeList.add(new SecurityConfig(role.getRoleKey()));
                    break;
                }
            }
        }

        return configAttributeList;
    }

    @Override
    public Collection<ConfigAttribute> getAllConfigAttributes() {
        return null;
    }

    @Override
    public boolean supports(Class<?> clazz) {
        return FilterInvocation.class.isAssignableFrom(clazz);
    }

}
