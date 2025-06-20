
```java
// ==== COMMON DTO ==== //

// OrderRequest.java (Shared between both services)
public class OrderRequest {
    private List<Long> productIds;

    // Getters and Setters
}

// =========================
// ==== PRODUCT SERVICE ====
// =========================

// Product.java
@Entity
public class Product {
    @Id @GeneratedValue
    private Long id;
    private String name;
    private BigDecimal price;
    // Getters and Setters
}

// ProductRepository.java
public interface ProductRepository extends JpaRepository<Product, Long> {}

// ProductService.java
@Service
public class ProductService {
    @Autowired private ProductRepository repo;

    public List<Product> getAll() {
        return repo.findAll();
    }
    public Product getById(Long id) {
        return repo.findById(id).orElseThrow();
    }
    public Product create(Product p) {
        return repo.save(p);
    }
}

// RabbitMQConfig.java
@Configuration
public class RabbitMQConfig {
    public static final String QUEUE = "order.queue";
    public static final String EXCHANGE = "order.exchange";
    public static final String ROUTING_KEY = "order.routing.key";

    @Bean
    public Queue queue() {
        return new Queue(QUEUE, true);
    }

    @Bean
    public DirectExchange exchange() {
        return new DirectExchange(EXCHANGE);
    }

    @Bean
    public Binding binding(Queue queue, DirectExchange exchange) {
        return BindingBuilder.bind(queue).to(exchange).with(ROUTING_KEY);
    }
}

// OrderPublisher.java
@Service
public class OrderPublisher {
    @Autowired private RabbitTemplate rabbitTemplate;

    public void sendOrder(OrderRequest orderRequest) {
        rabbitTemplate.convertAndSend(RabbitMQConfig.EXCHANGE, RabbitMQConfig.ROUTING_KEY, orderRequest);
    }
}

// ProductController.java
@RestController
@RequestMapping("/products")
public class ProductController {
    @Autowired private ProductService productService;
    @Autowired private OrderPublisher orderPublisher;

    @PostMapping
    public Product create(@RequestBody Product p) {
        return productService.create(p);
    }

    @GetMapping
    public List<Product> getAll() {
        return productService.getAll();
    }

    @PostMapping("/placeOrder")
    public String placeOrder(@RequestBody OrderRequest orderRequest) {
        orderPublisher.sendOrder(orderRequest);
        return "Order request sent!";
    }
}

// =======================
// ==== ORDER SERVICE ====
// =======================

// Order.java
@Entity
public class Order {
    @Id @GeneratedValue
    private Long id;
    private LocalDateTime createdAt;

    @ElementCollection
    private List<Long> productIds;
}

// OrderRepository.java
public interface OrderRepository extends JpaRepository<Order, Long> {}

// RabbitMQConfig.java (Same constants as Product Service)
@Configuration
public class RabbitMQConfig {
    public static final String QUEUE = "order.queue";
    public static final String EXCHANGE = "order.exchange";
    public static final String ROUTING_KEY = "order.routing.key";

    @Bean
    public Queue queue() {
        return new Queue(QUEUE, true);
    }

    @Bean
    public DirectExchange exchange() {
        return new DirectExchange(EXCHANGE);
    }

    @Bean
    public Binding binding(Queue queue, DirectExchange exchange) {
        return BindingBuilder.bind(queue).to(exchange).with(ROUTING_KEY);
    }
}

// OrderListener.java
@Service
public class OrderListener {
    @Autowired private OrderRepository orderRepository;

    @RabbitListener(queues = RabbitMQConfig.QUEUE)
    public void listen(OrderRequest request) {
        Order order = new Order();
        order.setCreatedAt(LocalDateTime.now());
        order.setProductIds(request.getProductIds());
        orderRepository.save(order);
        System.out.println("Order received and saved: " + order);
    }
}

```
