@Service
public class NotificationPanelServiceImpl implements NotificationPanelService {

    private final NotificationPanelRepository notificationPanelRepository;

    private final GroupUserRepository groupUserRepository;
    private final Keycloak keycloak;
    private final SmsClient smsClient;
    private final NotificationTemplateRepository notificationTemplateRepository;

    private final RabbitTemplate rabbitTemplate;
    private final FirebaseMessaging firebaseMessaging;

    private final TokenRepository tokenRepository;

    public NotificationPanelServiceImpl(
        NotificationPanelRepository notificationPanelRepository,
        GroupUserRepository groupUserRepository,
        Keycloak keycloak,
        SmsClient smsClient,
        NotificationTemplateRepository notificationTemplateRepository,
        RabbitTemplate rabbitTemplate,
        FirebaseMessaging firebaseMessaging,
        TokenRepository tokenRepository
    ) {
        this.notificationPanelRepository = notificationPanelRepository;
        this.groupUserRepository = groupUserRepository;
        this.keycloak = keycloak;
        this.smsClient = smsClient;
        this.notificationTemplateRepository = notificationTemplateRepository;
        this.rabbitTemplate = rabbitTemplate;

        this.firebaseMessaging = firebaseMessaging;
        this.tokenRepository = tokenRepository;
    }

public String sendNotificationByTokenAndRabit(NotificationRequet notificationRequet) {
        NotificationTemplate notificationTemplate = notificationTemplateRepository.findByType(notificationRequet.getType());
        Token token = tokenRepository.findByUserId(notificationRequet.getUserId());
        Map<String, String> data = new HashMap<>();
        data.put("message", notificationTemplate.getBody());
        data.put("title", notificationTemplate.getTitle());
        data.put("image", notificationTemplate.getImage());
        data.put("url", notificationTemplate.getUrl());
        Message message = Message.builder().setToken(token.getToken()).putAllData(data).build();

        try {
            firebaseMessaging.send(message);
            return "success";
        } catch (Exception e) {
            throw new RuntimeException();
        }
    }
}
